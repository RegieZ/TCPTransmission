import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class FileUpload_Server {
    public static void main(String[] args) throws IOException {
        ExecutorService service = Executors.newFixedThreadPool(2);
        // 1. 创建服务端ServerSocket
        ServerSocket serverSocket = new ServerSocket(9956);
        System.out.println("Server is set up ...");
        // 2. 循环接收,建立连接
        while (true) {
            Socket accept = serverSocket.accept();//socket要定义在while外面，否则就会堵塞
          	/*
          	3. socket对象交给子线程处理,进行读写操作
               Runnable接口中,只有一个run方法,使用lambda表达式简化格式
            */
            Thread t = new Thread(() -> {
                try (
                        //3.1 获取输入流对象
                        BufferedInputStream bis = new BufferedInputStream(accept.getInputStream());
                        //3.2 创建输出流对象, 保存到本地 .
                        FileOutputStream fis = new FileOutputStream(System.currentTimeMillis() + ".jpg");
                        BufferedOutputStream bos = new BufferedOutputStream(fis);
                ) {
                    // 3.3 读写数据
                    byte[] b = new byte[1024 * 8];
                    int len;
                    while ((len = bis.read(b)) != -1) {
                        bos.write(b, 0, len);
                    }

                    // 4.=======信息回写===========================
                    OutputStream out = accept.getOutputStream();
                    out.write("Server accepts upload.".getBytes());
                    out.close();
                    //================================

                    //5. 关闭 资源
                    bos.close();
                    bis.close();
                    accept.close();
                    System.out.println("Accept is over.");
                } catch (IOException e) {
                    e.printStackTrace();
                }
            });
            service.submit(t);
        }
    }
}
