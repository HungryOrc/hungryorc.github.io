---
title: "Design a Message Queue System having One Producer and Multiple Consumers"
tags: [algorithm, ood_design]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_design_message_queue_multiple_consumers.html
folder: algorithm
toc: false
---

## Description
要设计一个MQ系统。里面有1个producer和多个consumer。在这个MQ里，topic也就是queue(的name)可以有多个。特殊的要求是：
* 每个consumer只固定监听一个queue。它读取里面的msgs的顺序是先发布的msg必须被先读
* 这个queue里的所有msgs，这些consumer都要全部consume一遍，谁也不能少，哪个msg也不能少
* 所有的msgs都不删除，就算被所有的consumers都读完了以后
* 任何时候建立一个新的consumer，它都要能够读取这个queue的历史上从时间的起源开始的所有的msgs

什么时候produce，什么时候consume，哪个consumer来consume，produce和consume的topic，等等这些都不是我们来控制的。我们只要写出 produce 和 consume 的执行函数就行

要做到每次produce一个msg，或者consume一个msg，时间消耗都尽量小。比如说都是 O(1) ---- 下面这个方法做到了

### Example
略

## Solution：我自己的想法和实现

### Complexity
* Time: O(1) for both produce one msg and consume one msg
* Space: O(n * m)，n是consumers的个数，m是平均每个queue里的msgs的个数 <=== 对么？？？？

### Java
```java
class MQueue {
    // <topic, array list of msgs>
    Map<String, List<String>> msgQueues;
    
    // <topic, Map<consumerID, 当前consumer该读哪个msg了>>
    // 里面那个map的意思是说，关注一个topic的consumer可能有多个，但是对于一个topic来说，
    // 它过往produce过的msgs是一定的，且顺序固定的，那么每个consumer当前将要读到这个queue里的
    // 哪一个index所标示的msg，我么就要分别记录了
    Map<String, Map<Integer, Integer>> progressByConsumerByQueue;
    
    public MQueue() {
        msgQueues = new HashMap<>();
        progressByConsumerByQueue = new HashMap<>();
    }
    
    // 往MQ系统里加一个consumer，这个consumer的id是xx，且它固定听取queue的名字=topic
    public void addConsumer(int consumerID, String topic) {
        if (consumerID <= 0 || topic == null || topic.equals("")) return;
        
        Map<Integer, Integer> prog = progressByConsumerByQueue.get(topic);
        if (prog == null) {
            prog = new HashMap<>();
            progressByConsumerByQueue.put(topic, prog);
        }
        prog.put(consumerID, 0); // 0 表示这个consumer是新来的，它要从第一个msg开始读
        
        List<String> msgs = msgQueues.get(topic);
        if (msgs == null) {
            msgs = new ArrayList<>();
            msgQueues.put(topic, msgs);
        }
    }
    
    public void publishOneMsg(String topic, String message) {
        if (topic == null || topic.equals("") || message == null) return;
        
        List<String> msgs = msgQueues.get(topic);
        if (msgs == null) {
            msgs = new LinkedList<>();
            msgQueues.put(topic, msgs);
        }
        msgs.add(message);
    }
    
    public String consumeOneMsg(int consumerID, String topic) {
        if (consumerID <= 0 || topic == null || topic.equals("")) return null;
        
        List<String> msgs = msgQueues.get(topic);
        if (msgs == null || msgs.size() == 0) return null;
        
        Map<Integer, Integer> prog = progressByConsumerByQueue.get(topic);
        if (prog == null) return null;
        
        Integer indexOfMsgToRead = prog.get(consumerID);
        if (indexOfMsgToRead == null) return null;
        // 这个consumer已经把这个queue读完了
        if (indexOfMsgToRead == msgs.size()) return null;

        String msgToRead = msgs.get(indexOfMsgToRead);
        indexOfMsgToRead++;
        prog.put(consumerID, indexOfMsgToRead);
        
        return msgToRead;
    }
}
```

## Reference
苹果的电面题，网上没看到这题

{% include links.html %}
