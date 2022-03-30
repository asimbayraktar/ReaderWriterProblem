package sync;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Semaphore;


public class NewClass {
	public static void main(String [] args) {
		ExecutorService executorService = Executors.newCachedThreadPool();
		ReadWriteLock RW = new ReadWriteLock();
		
		
		executorService.execute(new Writer(1,RW));
		executorService.execute(new Writer(2,RW));
		executorService.execute(new Writer(3,RW));
		executorService.execute(new Writer(4,RW));
		
		executorService.execute(new Reader(1,RW));
		executorService.execute(new Reader(2,RW));
		executorService.execute(new Reader(3,RW));
		executorService.execute(new Reader(4,RW));	
	
		
	}
}


class ReadWriteLock{
	private Semaphore Sem_1=new Semaphore(1);
        private Semaphore Sem_2 = new Semaphore(1);
	private int readerCounter;
	public void readLock(int readerNo) {
	try{
         //mutual exclusion for readerCount 
            Sem_1.acquire();
         }
             catch (InterruptedException e) {}
      
         ++readerCounter;	
         
         if(readerCounter==1){
             try{
               Sem_2.acquire();
            }
                catch (InterruptedException e) {}
         }
            System.out.println("Reader " + readerNo + " is reading. Reader count = " + readerCounter);
            Sem_1.release();
                
		
	}
	public void writeLock(int writerNumber) {
	    try{
            Sem_2.acquire();
         }
             catch (InterruptedException e) {}
         System.out.println("Writer " + writerNumber + " is writing.");
		
	}
	public void readUnLock(int readerNo) {
	try{
         //mutual exclusion for readerCount
            Sem_1.acquire();
         }
             catch (InterruptedException e) {}
      
         --readerCounter;
         
         if(readerCounter == 0){
             Sem_2.release();
         }
         
            System.out.println("Reader " + readerNo + " is done reading. Reader Count = " + readerCounter);
            Sem_1.release();
		
		
	}
	public void writeUnLock(int writerNumber) {
        System.out.println("Writer " + writerNumber + " is done writing.");
        Sem_2.release();
		
		
	}

}




class Writer implements Runnable
{
   private ReadWriteLock RW_lock;
   private int writerNumber;
   
   
   

    public Writer(int writerNumber, ReadWriteLock rw) {
    	RW_lock = rw;
        this.writerNumber = writerNumber;
   }

    public void run() {
      while (true){
          SleepUtilities.nap();
         
        System.out.println("writer " + writerNumber + " wants to write.");
        RW_lock.writeLock(writerNumber);
    	  
    	SleepUtilities.nap();
    	RW_lock.writeUnLock(writerNumber);
       
      }
   }


}



class Reader implements Runnable
{
   private ReadWriteLock RW_lock;
   private int readerNo;
   

   public Reader(int readerNo,ReadWriteLock rw) {
    	RW_lock = rw;
        this.readerNo = readerNo;
   }
    public void run() {
      while (true){ 	  
          SleepUtilities.nap();
          
          System.out.println("reader " + readerNo + " wants to read.");
    	  RW_lock.readLock(readerNo);
          
          
          SleepUtilities.nap();
    	 
    	  
    	  RW_lock.readUnLock(readerNo);
       
      }
   }


}

class SleepUtilities
   {
   /**
    * Nap between zero and NAP_TIME seconds.
    */
       public static void nap() {
         nap(NAP_TIME);
      }
   
   /**
    * Nap between zero and duration seconds.
    */
       public static void nap(int duration) {
         int sleeptime = (int) (NAP_TIME * Math.random() );
         try { Thread.sleep(sleeptime*1000); }
             catch (InterruptedException e) {}
      }
   
      private static final int NAP_TIME = 5;
   }
