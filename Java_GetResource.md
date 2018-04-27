# 1. GetResource

Class.getResource can take a "relative" resource name, which is treated relative to the class's package. Alternatively you can specify an "absolute" resource name by using a leading slash. Classloader resource paths are always deemed to be absolute.

## 1.1 GetResource区别

- getClass().getResource() searches relative to the .class file 
- getClass().getClassLoader().getResource() searches relative to the classpath root.

## 1.2 Example

```java
/*
  SAMPLE OUTPUT:
  ClassLoader.getResource(/subdir/readme.txt): NULL
  Class.getResource(/subdir/readme.txt): SUCCESS

  ClassLoader.getResource(subdir/readme.txt): SUCCESS
  Class.getResource(subdir/readme.txt): NULL
 */
package com.so.resourcetest;

import java.net.URL;

public class ResourceTest {

    public static void main(String[] args) {
        ResourceTest app = new ResourceTest ();
    }

    public ResourceTest () {
        doClassLoaderGetResource ("/subdir/readme.txt");
        doClassGetResource ("/subdir/readme.txt");
        doClassLoaderGetResource ("subdir/readme.txt");
        doClassGetResource ("subdir/readme.txt");
    }

    private void doClassLoaderGetResource (String sPath) {
        URL url  = getClass().getClassLoader().getResource(sPath);
        if (url == null)
            System.out.println("ClassLoader.getResource(" + sPath + "): NULL");
        else
            System.out.println("ClassLoader.getResource(" + sPath + "): SUCCESS");
    }

    private void doClassGetResource (String sPath) {
        URL url  = getClass().getResource(sPath);
        if (url == null)
            System.out.println("Class.getResource(" + sPath + "): NULL");
        else
            System.out.println("Class.getResource(" + sPath + "): SUCCESS");
    }
}

```