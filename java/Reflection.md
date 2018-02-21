# Reflection

## 1.Constructor
```java
public static Object newInstance(Class clazz, Object... constructorArgs) {
    if (clazz == null) {
        return null;
    }

    Object object = null;

    int argLen = constructorArgs == null ? 0 : constructorArgs.length;
    Class<?>[] parameterTypes = new Class[argLen];
    for (int i = 0; i < argLen; i++) {
        parameterTypes[i] = constructorArgs[i].getClass();
    }

    try {
        Constructor constructor = clazz.getDeclaredConstructor(parameterTypes);
        if (!constructor.isAccessible()) {
            constructor.setAccessible(true);
        }
        object = constructor.newInstance(constructorArgs);

    } catch (Exception e) {
        e.printStackTrace();
    }

    return object;
}
```

## 2.invokeMethold

```java

public static Object invokeMethod(Object object, String methodName, Object... methodArgs) {
    if (object == null) {
        return null;
    }

    Object result = null;
    Class<?> clazz = object.getClass();
    try {
        Method method = clazz.getDeclaredMethod(methodName, obj2class(methodArgs));
        if (method != null) {
            if (!method.isAccessible()) {
                method.setAccessible(true);  //当私有方法时，设置可访问
            }
            result = method.invoke(object, methodArgs);
        }
    } catch (Exception e) {
        e.printStackTrace();
    }

    return result;

}

```

## 3.getField

```java
public static Object getField(Object object, String fieldName) {
    if (object == null) {
        return null;
    }

    Object result = null;
    Class<?> clazz = object.getClass();
    try {
        Field field = clazz.getDeclaredField(fieldName);
        if (field != null) {
            if (!field.isAccessible()) {
                field.setAccessible(true);
            }
            result = field.get(object);
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
    return result;
}
```

## 4.getStaticField

```java
public static boolean setStaticField(Class clazz, String fieldName, Object value) {
    if (clazz == null) {
        return false;
    }

    Object result = null;
    try {
        Field field = clazz.getDeclaredField(fieldName);
        if (field != null) {
            if (!field.isAccessible()) {
                field.setAccessible(true);
            }
            field.set(null, value);
        }
    } catch (Exception e) {
        e.printStackTrace();
        return false;
    }
    return true;
}
```

## 5.dumperClass

```java
public static String dumpClass(String className) {
    StringBuffer sb = new StringBuffer();
    Class<?> clazz;
    try {
        clazz = Class.forName(className);
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
        return "";
    }

    Constructor<?>[] cs = clazz.getDeclaredConstructors();
    sb.append("------ Constructor ------> ").append("\n");
    for (Constructor<?> c : cs) {
        sb.append(c.toString()).append("\n");
    }

    sb.append("------ Field ------>").append("\n");
    Field[] fs = clazz.getDeclaredFields();
    for (Field f : fs) {
        sb.append(f.toString()).append("\n");
        ;
    }
    sb.append("------ Method ------>").append("\n");
    Method[] ms = clazz.getDeclaredMethods();
    for (Method m : ms) {
        sb.append(m.toString()).append("\n");
    }
    return sb.toString();
}

```