```
import com.javaSE.jichengDuotai.Father;
import org.openjdk.jol.info.ClassLayout;

/**
 * @Author wuzhuoqi
 * @Date 2020/11/25 9:52
 */
public class LockAndMarkWordSize {

    public static void main(String[] args) throws InterruptedException {
        //haveyWeightLock();
        //lightWeightLock2();
        //lightWeightLock();
        lightWeightLock();

    }

    //直接创建对象  无锁 001
    public static void noLock(){
        Father father = new Father();
        System.out.println(ClassLayout.parseInstance(father).toPrintable());
    }

    //jvm默认开启偏向锁延迟 4-5s  所以线程等待5s ,再创建对象 ,加了没有线程id的偏向锁 101
    public static void biasedLock(){
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        Father father = new Father();
        System.out.println(ClassLayout.parseInstance(father).toPrintable());
    }

    //jvm默认开启偏向锁延迟 4-5s  所以线程等待5s ,再创建对象并加锁 ,加了带有线程id的偏向锁 101
    public static void biasedLock2(){
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        Father father = new Father();
        synchronized (father){
            System.out.println(ClassLayout.parseInstance(father).toPrintable());
        }
    }


    //jvm默认开启偏向锁延迟 4-5s  所以线程等待5s ,再创建对象并加锁 ,加了带有线程id的偏向锁 101
    //调用对象的hashCode方法 , 会清除对象的偏向锁 , 变成无锁状态 001
    //后续对该对象再加锁 , 只能加轻量级锁 或者 重量级锁   00 或 10
    public static void biasedLock3(){
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        Father father = new Father();
        synchronized (father){
            System.out.println(ClassLayout.parseInstance(father).toPrintable());
        }
        father.hashCode();
        System.out.println(ClassLayout.parseInstance(father).toPrintable());
        synchronized (father){
            System.out.println(ClassLayout.parseInstance(father).toPrintable());
        }

    }


    //t1 t2 对同一对象加锁 , 但是没有争抢 或争抢较轻 , 所以升级为轻量级锁 00
    public static void lightWeightLock() throws InterruptedException {
        Father father = new Father();
        Thread t1 = new Thread(()->{
            synchronized (father){
                System.out.println(ClassLayout.parseInstance(father).toPrintable());
            }
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        Thread t2 = new Thread(()->{
            synchronized (father){
                System.out.println(ClassLayout.parseInstance(father).toPrintable());
            }
        });

        t1.start();
        Thread.sleep(2000);
        t2.start();
    }

    //对象创建后,直接加锁 , 就是轻量级锁 00
    /*
    原因 : JVM启动时, 前4秒带synchronized的方法或代码块 , 上的是轻量级锁 , 原因是JVM启动过程中 , 本身运行了很多带synchronized的方法,
    而且这些方法都是轻量级锁 , 如果直接设置成偏向锁 , 那么在锁升级过程中 , 偏向锁清除锁的过程比较复杂 , 导致锁升级消耗的资源比较多 ,
    所以默认开启了延迟偏向锁 .
    * */
    public static void lightWeightLock2() throws InterruptedException { 
        Father father = new Father();
        synchronized (father){
            System.out.println(ClassLayout.parseInstance(father).toPrintable());
        }
    }


    //两个线程同时启动 并对同一对象加锁 , 直接升级为重量级锁 10
    public static void haveyWeightLock() throws InterruptedException {
        Father father = new Father();
        Thread t1 = new Thread(() -> {
            synchronized (father) {
                System.out.println(ClassLayout.parseInstance(father).toPrintable());
            }
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        Thread t2 = new Thread(() -> {
            synchronized (father) {
                System.out.println(ClassLayout.parseInstance(father).toPrintable());
            }
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        t1.start();
        t2.start();
    }
}
```