##自定义ClassLoad

```java
package com.test.classload.demo;

import java.io.*;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;


public class MyClassLoader extends ClassLoader{

    private static final String HELLO_CLASS_FILE = "./Hello.xlass";
    private static final String HELLO_CLASS_NAME = "Hello";
    private static final String HELLO_CLASS_METHOD = "hello";


    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException {
        final Class<?> aClass = new MyClassLoader().findClass(HELLO_CLASS_NAME);
        final Object obj = aClass.newInstance();

        Method method = null;
        try {
            method = aClass.getMethod(HELLO_CLASS_METHOD);
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        }
        try {
            method.invoke(obj);
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
    }

    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {


        try {
            byte[] fileContent = getFileContent(HELLO_CLASS_FILE);
            return defineClass(name,fileContent,0,fileContent.length);

        } catch (IOException e) {
            e.printStackTrace();
        }

        return super.findClass(name);

    }


    public static byte[] getFileContent(String filePath) throws IOException {



        int buf_size = 1024;
        byte[] bytes = new byte[buf_size];
        final File file = new File(filePath);

        final ByteArrayOutputStream bos = new ByteArrayOutputStream((int) file.length());
        BufferedInputStream in = null;

        try {
            in = new BufferedInputStream(new FileInputStream(file));

            int len = 0;
            while (-1 != (len = in.read())) {
                len = 255 -len;
                bos.write(len);
            }
            bytes = bos.toByteArray();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                bos.close();
                in.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        return bytes;
    }
}
```

##Xmx、Xms、Xmn、Meta、DirectMemory、Xss 这些内存参数的关系

![jvm结构和设置](/Users/zhaiwei/work_dir/JAVA-000/tree/main/Week_01/jvm结构和设置.png)