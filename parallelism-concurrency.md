# Parallelism vs Concurrency

## Introduction
A few days ago, I was interviewing a candidate for a Backend position at my current company ([Coverwallet](https://www.coverwallet.com/jobs/tech)) along with one of the seniors developers on the team. While we were on the system architecture problem, we discussed an approach that involved making independent requests to multiple 3rd parties, to which the candidate told us that he would use parallel requests. This quickly triggered a question by my colleague: "Parallel or concurrent?"

I have to admit that, up to that point, I had no idea that those terms had different meanings. Neither had the candidate, so my colleague gave us a brief explanation about each one and its similitudes and differences. After that, the candidate commented that now that he knew that, he would in fact use a concurrent approach and justified why, and then we followed the interview. 

I had to admit that small discussion piqued my curiosity, and that's why I've decided to write this article.

## Parallel and Concurrent
Of all the definitions that I've found, the one I've liked the most is this one that we can found on the book [_The Art of Concurrency_](https://www.amazon.com/Art-Concurrency-Monkeys-Parallel-Applications-ebook/dp/B002L4EXD8/ref=as_li_ss_tl?ie=UTF8&qid=1524421923&sr=8-1&keywords=the+art+of+concurrency&linkCode=ll1&tag=algasbo-21&linkId=a0b296b6d22fbb99d1127d966c5b9b46)

> A system is said to be concurrent if it can support two or more actions _in progress_ at the same time. A system is said to be parallel if it can support two or more actions executing _simultaneously_. The key concept and difference between these definitions is the phrase "in progress."

This is: while two parallel actions have to do their work and processing at the same time, the processing of an action concurrent to another one can happen while the latter is blocked. 

An easy way to understand this is with something that probably we do on a daily basis: imagine that you want to check both your mail and your Facebook account. Doing it in parallel would require you to have two different devices (for example, your laptop and your smartphone) and to check each site with one of them, exactly at the same time. On the other hand, you could do it concurrently if you use different tabs of the same browser: you open Facebook, and while it's loading your feed, you go to a new tab and open GMail; you quickly see that you don't have any new e-mail, close the tab and go back to Facebook. At that point, your feed has finished loading and therefore you can enjoy its content.

![concurrent-parallel-image](https://techdifferences.com/wp-content/uploads/2017/12/Untitled.jpg)

By using the concurrent approach, you avoided having to wait without doing anything while both pages where loading, and also reduced the amount of resources needed to do the task. This is why concurrent processes are widely used when doing web requests, because most of the time you're just waiting for the destination server to respond. 
