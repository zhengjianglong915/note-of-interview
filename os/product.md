## 背景
生产者消费者模式是通过一个容器来解决生产者和消费者的强耦合问题。生产者和消费者彼此之间不直接通讯，而通过阻塞队列来进行通讯，所以生产者生产完数据之后不用等待消费者处理，直接扔给阻塞队列，消费者不找生产者要数据，而是直接从阻塞队列里取，阻塞队列就相当于一个缓冲区，平衡了生产者和消费者的处理能力。解决生产者/消费者问题的方法可分为两类：

- （1）采用某种机制保护生产者和消费者之间的同步；
- （2）在生产者和消费者之间建立一个管道。第一种方式有较高的效率，并且易于实现，代码的可控制性较好，属于常用的模式。第二种管道缓冲区不易控制，被传输数据对象不易于封装等，实用性不强。因此本文只介绍同步机制实现的生产者/消费者问题。

## 实现方式
在Java中一共有四种方法支持同步，其中前三个是同步方法，一个是管道方法。

- 1) wait() / notify()方法
- 2) await() / signal()方法
- 3) BlockingQueue阻塞队列方法
- 4) PipedInputStream / PipedOutputStream


## wait()/notify()



### blockingQueue方式

    public class ProducerConsumer<T> {
        static  class MsgQueueManager<T> {
            /**
             * 消息总队列
             */
            public final BlockingQueue<T> messageQueue;
            MsgQueueManager(BlockingQueue<T> messageQueue) {
                this.messageQueue = messageQueue;
            }
            MsgQueueManager() {
                this.messageQueue = new LinkedBlockingQueue<T>();
            }
    
            public void put(T msg) {
                try {
                    messageQueue.put(msg);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
    
            public T take() {
                try {
                    return messageQueue.take();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
                return null;
            }
        }
    
        static  class Producer extends Thread {
            private MsgQueueManager msgQueueManager;
            public Producer(MsgQueueManager msgQueueManager) {
                this.msgQueueManager = msgQueueManager;
            }
            @Override
            public void run() {
                msgQueueManager.put(new Object());
            }
        }
        static class Consumer extends Thread{
            private MsgQueueManager msgQueueManager;
            public Consumer(MsgQueueManager msgQueueManager) {
                this.msgQueueManager = msgQueueManager;
            }
            @Override
            public void run() {
                Object o = msgQueueManager.take();
            }
        }
    
        public static void main(String[] args) {
            MsgQueueManager<String> msgQueueManager = new MsgQueueManager<String>();
    
            for(int i = 0; i < 100; i ++) {
                new Producer(msgQueueManager).start();
    
            }
            for(int i = 0; i < 100; i ++) {
                new Consumer(msgQueueManager).start();
    
            }
        }
    }



http://www.infoq.com/cn/articles/producers-and-consumers-mode/
http://blog.csdn.net/monkey_d_meng/article/details/6251879


