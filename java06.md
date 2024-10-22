# 2024091203030-吴芳玉-JAVA-06
## Task1
### Dish
```java
package com.glimmer;

public class Dish {
    private String name;
    private double price;
//无参构造器
    public Dish() {
    }
//有参构造器
    public Dish(String name, double price) {
        this.name = name;
        this.price = price;
    }
//get，set方法
    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
//cook方法
    public void cook(){

    }
//profile方法，用于子类重写
    public void profile(){

    }
}
```
### Dish_1
```java
package com.glimmer;


public class Dish_1 extends Dish {
   public Dish_1(){
       super("香酥鸭",88.0);
   }

    @Override
    public void profile() {
        System.out.println("你好香酥鸭！");

    }
    
}
```
### Dish_2
```java
package com.glimmer;

public class Dish_2 extends Dish  {
   public Dish_2(){
       super("麻辣小龙虾",99.0);
   }

    @Override
    public void profile() {
        System.out.println("你好麻辣小龙虾！");
    }
    
}

```
## Task2
### Order接口
```java
package com.glimmer;

import java.util.Random;

public interface Order {
    //用于重写
     void cook();

    //用于生成随机布尔值
     default boolean check(){
         return new Random().nextBoolean();
     };
}
```
### Dish_1及其对Order的调用
```java
package com.glimmer;

import java.util.Random;

public class Dish_1 extends Dish implements Order{
   public Dish_1(){
       super("香酥鸭",88.0);
   }

    @Override
    public void profile() {
        System.out.println("你好香酥鸭！");

    }

    @Override
    public void cook() {
        System.out.println("香酥鸭的烹饪方法是......");
    }
//用于返回布尔值判断原料是否够用
    @Override
    public boolean check() {
        return check();
    }
}
```
### Dish_2及其对Order的调用
```java
package com.glimmer;

import java.util.Random;

public class Dish_2 extends Dish implements Order {
   public Dish_2(){
       super("麻辣小龙虾",99.0);
       System.out.println("名字"+getName()+"价格"+getPrice());
   }

    @Override
    public void profile() {
        System.out.println("你好麻辣小龙虾！");
    }

    @Override
    public void cook() {
        System.out.println("麻辣小龙虾的烹饪方法是.....");
    }
//用于返回布尔值判断原料够不够用
    @Override
    public boolean check() {
        return check();
    }
}

```
### 以下是厨师类cookSystem（由于以System命名会与预定义的System类冲突，故将名称改为了cookSystem）
```java
package com.glimmer;

import java.util.ArrayList;
import java.util.List;

public class cookSystem {
    private static int i =1;//订单编号初始值
    public void manageOrder(List<Order> dishes){
        boolean enough = true;
        //判断是否需要取消订单
        for(Order dish:dishes){
            if(dish.check()==false) {
                System.out.println("取消订单");
                enough = false;
                break;
            }
        }
        //筛选掉取消的订单后，遍历所有菜品中的cook方法
        if(enough == true) {
            for (Order dish : dishes) {
                dish.cook();
            }
            System.out.println("该订单的编号是" + i);
            //订单编号递增
            i++;
        }
    }
}

```
## Task3
### 以下是完善后的System类
```java
package com.glimmer;

import java.util.ArrayList;
import java.util.List;

public class cookSystem  {
    private static int i =1;
    //增加一个customers参数用于判断其是TableCustomer还是WechatCustomer的实例
    public <E>void manageOrder(List<Order> dishes, Object customers){
        boolean enough = true;
        //判断是否需要取消订单
        for(Order dish:dishes){
            if(dish.check()==false) {
                System.out.println("取消订单");
                enough = false;
                break;
            }
        }
        //筛选掉取消的订单后，遍历所有菜品中的cook方法
        if(enough) {
            for (Order dish : dishes) {
                dish.cook();
            }
            System.out.println("该订单的编号是" + i);
            //对TableCustomer和WechatCustomer两类顾客做出对应的行为
            if(customers instanceof TableCustomer){
                TableCustomer tableCustomers = (TableCustomer) customers;
                System.out.println("送餐到"+tableCustomers.tableId+"号桌子上");
            } else if (customers instanceof WechatCustomer) {
                WechatCustomer wechatCustomers = (WechatCustomer) customers;
                if(wechatCustomers.takeout){
                    System.out.println("外卖到"+wechatCustomers.address);
                }else {
                    System.out.println("堂食");
                }

            }
            //订单编号递增
            i++;
        }


    }
}

```