# python reference fragments

This paper used to record the common python fragment

python concurrent processing small frame

```
#!/usr/bin/env python
import Queue
import sys
import threading
import commands
 
class MutiThread(threading.Thread):
    def __init__(self,target_queue,run_job,r_queue):
        self.tq = target_queue
        self.rb = run_job
        self.rq = r_queue
        threading.Thread.__init__(self)
 
    def run(self):
        while True:
            try:
                tname = self.tq.get(False)
                self._process_job(tname)
            except Queue.Empty:
                break
    def _process_job(self,target):
        self.rq.put(self.rb['m_fun'](target))
 
class MutiRunner():
    def __init__(self,target_queue,run_job,concurrency):
        self.concurrency=concurrency
        self.target_queue=target_queue
        self.run_job=run_job
        self.resultqueue=Queue.Queue(0)
        self.total=self.target_queue.qsize()
    def start(self):
        for i in range(1,self.concurrency):
              MutiThread(self.target_queue,self.run_job,self.resultqueue).start()
        while self.total>0:
            try:
                ro=self.resultqueue.get()
                self.total=self.total-1
                self.run_job['r_fun'](ro)
            except Queue.Empty:
                break
 
def domd5sum(target):
    res=commands.getstatusoutput("md5sum "+target)
    return res
 
def reader(target):
    print target[1]
 
if __name__=='__main__':
    diskname=sys.argv[1]
    runqueue=Queue.Queue(0)
    for i in commands.getoutput("find /"+diskname+"/data/ -name \"blk*\" | grep  meta").split('\n'):
        runqueue.put(i)
    mr=MutiRunner(runqueue,{'m_fun':domd5sum,'r_fun':reader},2)
    mr.start()
```