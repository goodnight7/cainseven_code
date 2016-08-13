[TOC]

# 用到的几个关键函数 

## 输入流转字节数组
```
    public static byte[] inputStream2Bytes(InputStream is) throws IOException {

        ByteArrayOutputStream bytestream = new ByteArrayOutputStream();
        byte[] buffer = new byte[1024];
        int ch;
        while ((ch = is.read(buffer)) != -1) {
            bytestream.write(buffer, 0, ch);
        }
        byte[] data = bytestream.toByteArray();
        bytestream.close();
        return data;

    }
```

## 字节数组转文件

```
public static void bytes2File(byte[] b, String path) throws IOException {


        FileImageOutputStream imageOutput = new FileImageOutputStream(new File(path));
        imageOutput.write(b, 0, b.length);
        imageOutput.close();


    }
```

## 输入流转字符串

        基于64位编码 将字节数组编码成字符串

```
import org.apache.commons.net.util.Base64;

    public static String inputStream2StringBase64(InputStream inputStream) {
        String s = null;
        try {

            byte[] bytes = inputStream2Bytes(inputStream);
            s = Base64.encodeBase64String(bytes).replaceAll("\r\n", "");
        } catch (IOException e) {
            e.printStackTrace();
        }
        return s;
    }

```