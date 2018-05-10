package thread_test;

import java.util.Random;
import java.util.concurrent.locks.ReentrantLock;

import org.omg.Messaging.SyncScopeHelper;

public class thread_test {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//本次为两种资源的分配	请求资源用random函数生成
		int reqsour1 ;
      	int reqsour2 ;
      	Random r = new Random();
		source s = new source();
		
		//线程个数自己决定
		Thread[] threads = new Thread[100];
		  for (int i = 0; i < 100; i++) { 
	        	reqsour1 = r.nextInt(10);
	        	reqsour2 = r.nextInt(10); 
	            Thread thread = new test(s,i,reqsour1,reqsour2);//创建多个资源使用者    
	            threads[i] = thread;    
	        }    
	        for(int i = 0; i < 100; i++){    
	            Thread thread = threads[i];    
	            try {    
	                thread.start();//启动线程    
	            }catch (Exception e){    
	                e.printStackTrace();    
	            }    
	        }
	}
}

//资源类
class source
{
	final ReentrantLock lock = new ReentrantLock(true);
	//资源多少自己分配   个数也是自己分配
	int i=10;
	int j=10;
	public boolean test(int id,int reqsour1,int reqsour2)
	{
			boolean b =false;
			//此处资源未处理完    另一个线程就开始判断
			//比如第一个拿着lock的    对资源处理一段时间后暂停 ，第二个就判断是否够资源（本来应该是第一个拿着lock的完全处理完资源才判断的）
			
			//对信号减需要同步锁
				lock.lock();						
					if(i>=reqsour1&&j>=reqsour2)		
					{
						i-=reqsour1;
						j-=reqsour2;
						System.out.println("取： "+id+" "+"i:"+i+" j:"+j);	
					}
					else{//如果资源分配不合理  则回滚 while循环
						lock.unlock(); return b;}
				lock.unlock();
				b=true;
				
				//对信号恢复需要同步锁
				lock.lock();
					//资源释放
					j+=reqsour2;
					i+=reqsour1;
					System.out.println("放: "+id+" "+"i:"+i+" j:"+j);
				lock.unlock();
			return b;
	}
}


class test extends Thread
{
	boolean b=false;
	int reqsour1;
	int reqsour2;
	int id;
	source s;
	
	//分配函数： 参数： 对象s 线程id 请求资源1  请求资源2
	public test(source s,int id,int reqsour1,int reqsour2)
	{
		this.id=id;
		this.s=s;
		this.reqsour1=reqsour1;
		this.reqsour2=reqsour2;
	}
	public void run()
	{
		//System.out.println("id:"+id+" reqsour1: "+reqsour1+" reqsour2: "+reqsour2);
		//因为请求资源可能会大于此刻的资源数   所以得设置循环 来回滚
		while(true){
		b=s.test(id,reqsour1,reqsour2);
		
		//当b=true时  表示一次分配成功，不需要回滚
		if(b)
		{
			break;
		}
		//分配失败   等待100毫秒（等待时间自己测试   100不是最好的）  回滚  
		else{
			System.out.println(id+" 不合理分配  正在等待资源");
			try {
				Thread.sleep(100);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			}
		}
		
	}
}
