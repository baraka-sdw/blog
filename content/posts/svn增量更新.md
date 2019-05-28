---
title: 利用svn补丁，打增量包
date: 2018-04-10 17:20:11
categories: ["实用技巧"]
tags: ["svn","增量包"]
featured_image: https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/bing_photos/20180410.jpg
---

<p class="description">每日一句:Remember what should be remembered, and forget what should be forgotten.Alter what is changeable, and accept what is mutable.</p>

<!-- more -->

### 先创建补丁
>![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/20180426.PNG)


### 生成增量包代码
``` java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;

public class FreePatchUtil 
{

	public static String projectPath = "F:/BANKSERVERS/Ticm/TicmHB/src/main/java";	//代码目录
	public static String projectPath2 = "F:/BANKSERVERS/Ticm/TicmHB/WebRoot";	//webRoot目录
	public static String classPath = "F:/BANKSERVERS/Ticm/.metadata/.plugins/org.eclipse.wst.server.core/tmp0/wtpwebapps/TicmHB/WEB-INF/classes";//项目编译后的classes目录
	public static String patchFile = "F:/patch/patch.txt";	//升级补丁描述文件路径
	public static String desPath = "F:/patch/update";	//打包后升级包补丁包存放目录
	public static String version = "20180426";	//补丁版本号
	
	
	/**
	 * @param args
	 * @throws Exception 
	 */
	public static void main(String[] args) throws Exception 
	{
		copyFiles(getPatchFileList());
	}
	
	/**
	 * 分析svn补丁文件
	 * @return
	 * @throws Exception
	 */
	public static List<String> getPatchFileList() throws Exception
	{
		List<String> fileList=new ArrayList<String>();
		FileInputStream f = new FileInputStream(patchFile); 
		BufferedReader dr = new BufferedReader(new InputStreamReader(f,"utf-8"));
		String line;
		while((line=dr.readLine())!=null)
		{ 
			//eclipse svn create path 创建的补丁包信息
			if(line.indexOf("Index:")!=-1)
			{
				line=line.replaceAll(" ","");
				line=line.substring(line.indexOf(":")+1,line.length());
				fileList.add(line);
			}
			//如果是文件夹, 则跳过
			else if(line.indexOf(".") == -1)
			{
				continue;
			}
			//TortoiseSVN show log 复制的补丁包信息
			else if(line.startsWith("/api/"))
			{
				line = line.substring("/api/".length());
				fileList.add(line);
			}
		} 
		return fileList;
	}
	
	/**
	 * 根据不同资源, 拷贝到补丁包的不同路径
	 * @param list
	 */
	public static void copyFiles(List<String> list)
	{
		for(String fullFileName:list)
		{
			//复制classes文件
			if(fullFileName.indexOf("src/main/java") != -1)
			{
				String fileName = fullFileName.replace("src/main/java", "");
				fullFileName = classPath + fileName;
				
				if(fileName.endsWith(".java"))
				{
					fileName = fileName.replace(".java",".class");
					fullFileName = fullFileName.replace(".java",".class");
				}
				
				String desFilePathStr = desPath + "/" + version + "/WEB-INF/classes" + fileName.substring(0, fileName.lastIndexOf("/"));
				String desFileNameStr = desPath + "/" + version + "/WEB-INF/classes" + fileName;
				
				File desFilePath=new File(desFilePathStr);
				if(!desFilePath.exists())
				{
					desFilePath.mkdirs();
				}
				copyFile(fullFileName, desFileNameStr);
				System.out.println(fullFileName+"复制完成");
			}
			//复制资源配置文件
			else if(fullFileName.indexOf("src/main/config") != -1)
			{
				String fileName = fullFileName.replace("src/main/config", "");
				fullFileName = classPath + fileName;
				
				String desFilePathStr = desPath + "/" + version + "/WEB-INF/classes" + fileName.substring(0, fileName.lastIndexOf("/"));
				String desFileNameStr = desPath + "/" + version + "/WEB-INF/classes" + fileName;
				
				File desFilePath=new File(desFilePathStr);
				if(!desFilePath.exists())
				{
					desFilePath.mkdirs();
				}
				copyFile(fullFileName, desFileNameStr);
				System.out.println(fullFileName+"复制完成");
			}
			//复制web资源
			else if (fullFileName.indexOf("WebRoot") != -1) 
			{
				String fileName = fullFileName.replace("WebRoot", "");
				fullFileName = projectPath2 + fileName;
				
				String desFilePathStr = desPath + "/" + version + fileName.substring(0, fileName.lastIndexOf("/"));
				String desFileNameStr = desPath + "/" + version + fileName;
				
				File desFilePath=new File(desFilePathStr);
				if(!desFilePath.exists())
				{
					desFilePath.mkdirs();
				}
				copyFile(fullFileName, desFileNameStr);
				System.out.println(fullFileName+"复制完成");
			}
		}
	}

	private static void copyFile(String sourceFileNameStr, String desFileNameStr) {
		File srcFile=new File(sourceFileNameStr);
		File desFile=new File(desFileNameStr);
		try {
			copyFile(srcFile, desFile);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	
	public static void copyFile(File sourceFile, File targetFile) throws IOException {
        BufferedInputStream inBuff = null;
        BufferedOutputStream outBuff = null;
        try {
            // 新建文件输入流并对它进行缓冲
            inBuff = new BufferedInputStream(new FileInputStream(sourceFile));

            // 新建文件输出流并对它进行缓冲
            outBuff = new BufferedOutputStream(new FileOutputStream(targetFile));

            // 缓冲数组
            byte[] b = new byte[1024 * 5];
            int len;
            while ((len = inBuff.read(b)) != -1) {
                outBuff.write(b, 0, len);
            }
            // 刷新此缓冲的输出流
            outBuff.flush();
        } finally {
            // 关闭流
            if (inBuff != null)
                inBuff.close();
            if (outBuff != null)
                outBuff.close();
        }
    }
	
}
```

### <font size=4 color= red>注意: 该代码对内部类处理有问题，遇到内部类需要手动处理！！！！！</font>

