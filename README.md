# 2023-Spring-Summary
## CMU 15-445/645
### PROJECT 1 - Bufferpool Manager
>1.LRU-K Based cache replacement policy  
>>For storing K-History, using std::map or std::set, so that you can get a better **GET** QPS which weights a lot in Leaderboard grading.  
>>For K, the paper recommands to set k as **2**.  
>>Never use raw pointer! Using std::shared_ptr or std::unique_ptr instead. Or you can use std::container to store them temporarily.  
>2.BufferPoolManager Interfaces  
>>You may notice that there is a [[may be unused]] param, maybe you can seperate Scan's K-History storage and Get's K-History by using this param.  
>3.PageGuard  
>>Which is significantly important in B+tree index(Project 2).  

### PROJECT 2 - B+Tree Index
>1.RAII  
>>Using PageGuard instead of raw page obj.  
>2.Crabbing Lock Protocol  
>>Improve your concurrency execution.  
>3.Binary Search  
>>Searching strategy are different in Internal Page obj and Leaf Page obj.  
>4.Optimistic Locks  
>>This optimise may not obvise due to the shape of B+Trees are chunky.  
>5.Be Patient  
>>This Project can be tricky because of the complexity of the algorithm, chill and peace, you have suffered.  

### PROJECT 3 - Query Executors
>1.Iterator Model  
>>Children executors push its results to their parent.  
>2.Code Reading techniques  
>>Using Github and refer to the executors are compeleted, like filter executor, projection-executor.  
>>Using GDB to check the stack & heap, so that you can clearly see the topology of Iterator model.  
>3.Predicate Pushdown  
>>This is a TODO job.  
>4.In-memory Join  
>>Joins are assumed that data sets are fit in memory.  

### PROJECT 4 - Concurrency Control
>1.Two Phase Locking  
>2.Mock Lock  
>>Conbining std::unique_lock and std::condition_variable  
```C++
std::mutex mu;
std::unique_lock<std::mutex> lock(&mu);
std::condition_variable cv;
cv.wait(lock, [&](){
  bool some_panding_condition;
  if (some_panding_condition) {
    return false;
  }
  return true;
});


// in another thread
cv.notify_all();
```
>3.Concurrency in C++
```C++
std::vector<std::thread> tasks;
std::mutex mu;
auto task = [&](int32_t arg){
  std::lock_guard<std::mutex> guard(&mu);
  std::cout << arg << std::endl;
};
for(int i = 0; i < 10; ++i){
  tasks.emplace_back(task, i);
}

for(int i = 0; i < 10; ++i){
  tasks[i].join();
}
```
