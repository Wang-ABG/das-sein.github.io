
![页面提取自－13、 设计模式](https://github.com/user-attachments/assets/da58bed9-5a73-4630-aed5-ae56534cca64)
[13、 设计模式.pdf](https://github.com/user-attachments/files/17894059/13.pdf)

## 第一篇：23种设计模式

23种设计模式是面向对象编程中常用的设计思想，它们被分为三大类：创建型模式、结构型模式和行为型模式。以下是这三种模式及其具体分类的详细介绍：

### 1. 创建型模式（Creational Patterns）

创建型模式关注于对象的创建过程，旨在通过不同的方式来创建对象，从而增加代码的灵活性和复用性。

- **工厂模式（Factory Pattern）**：定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂方法让类的实例化推迟到子类中进行。具体包括简单工厂模式、工厂方法模式和抽象工厂模式。
- **单例模式（Singleton Pattern）**：确保一个类仅有一个实例，并提供一个全局访问点。
- **建造者模式（Builder Pattern）**：将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。
- **原型模式（Prototype Pattern）**：用原型实例指定创建对象的种类，并且通过复制这些原型来创建新的对象。

### 2. 结构型模式（Structural Patterns）

结构型模式关注于类与类之间的组合关系，通过组合不同的类或对象来获得更大的结构。

- **适配器模式（Adapter Pattern）**：将一个类的接口转换成客户端所期待的另一种接口形式，从而使原本因接口不匹配而无法一起工作的类能够一起工作。
- **桥接模式（Bridge Pattern）**：将抽象部分与实现部分分离，使它们都可以独立地变化。
- **过滤器模式（Filter Pattern）**：注意，这个模式在GoF的23种经典设计模式中并未直接列出，但类似的思想在如责任链模式等中有所体现。在结构型模式的范畴内，可以视为一种通过链式处理请求的结构模式。
- **组合模式（Composite Pattern）**：将对象组合成树形结构以表示“部分-整体”的层次结构，使得用户对单个对象和组合对象的使用具有一致性。
- **装饰器模式（Decorator Pattern）**：动态地给一个对象添加一些额外的职责，就增加功能来说，装饰器模式比生成子类更加灵活。
- **外观模式（Facade Pattern）**：为子系统中的一组接口提供一个一致的界面，此模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。
- **享元模式（Flyweight Pattern）**：运用共享技术有效地支持大量细粒度的对象。
- **代理模式（Proxy Pattern）**：为其他对象提供一种代理以控制对这个对象的访问。

### 3. 行为型模式（Behavioral Patterns）

行为型模式关注于对象之间的交互和职责分配，以更好地设计软件的行为。

- **责任链模式（Chain of Responsibility Pattern）**：使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。
- **命令模式（Command Pattern）**：将一个请求封装为一个对象，从而使你可用不同的请求对客户进行参数化；对请求排队或记录请求日志，以及支持可取消的操作。
- **解释器模式（Interpreter Pattern）**：给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子。
- **迭代器模式（Iterator Pattern）**：提供一种方法顺序访问一个聚合对象中的各个元素，而又不暴露该对象的内部表示。
- **中介者模式（Mediator Pattern）**：用一个中介对象来封装一系列的对象交互。中介者使各个对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。
- **备忘录模式（Memento Pattern）**：在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可将该对象恢复到原先保存的状态。
- **观察者模式（Observer Pattern）**：定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。
- **状态模式（State Pattern）**：允许一个对象在其内部状态改变时改变它的行为。对象看起来似乎修改了它的类。
- **空对象模式（Null Object Pattern）**：注意，这个模式并非GoF的23种经典设计模式之一，但它是面向对象编程中常用的设计模式之一，用于解决在调用可能返回null的对象方法时可能出现的空指针异常问题。
- **策略模式（Strategy Pattern）**：定义一系列的算法，把它们一个个封装起来，并且使它们可相互替换。此模式让算法的变化独立于使用算法的客户。
- **模板方法模式（Template Method Pattern）**：定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。
- **访问者模式（Visitor Pattern）**：表示一个作用于某对象结构中的各元素的操作。它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作。

这23种设计模式在软件开发中被广泛使用，通过应用这些模式可以提高代码的可复用性、灵活性、可扩展性等方面的特性，帮助开发人员更快地构建出高质量的软件系统。

## 第二篇：常用的设计模式

### **工厂模式 (Factory Pattern)**

**(1) 目的：** 提供一个接口或方法，负责创建特定类的实例，而不公开实例化的具体逻辑，帮助解耦对象的创建和使用。
**(2) 编码场景：**

- 当需要控制对象创建过程（如初始化某些状态）。
- 例如：支付系统中，按支付方式（微信、支付宝、银行卡）创建支付处理对象，通过工厂方法返回特定的支付类。

 **(3) 代码示例：**

```java
// 抽象产品
interface Payment {
    void pay();
}

// 具体产品
class WeChatPay implements Payment {
    public void pay() {
        System.out.println("WeChat Payment");
    }
}
class AliPay implements Payment {
    public void pay() {
        System.out.println("AliPay Payment");
    }
}

// 工厂类
class PaymentFactory {
    public static Payment createPayment(String type) {
        if (type.equals("WeChat")) return new WeChatPay();
        else if (type.equals("Ali")) return new AliPay();
        else throw new IllegalArgumentException("Unknown Payment Type");
    }
}

// 调用代码
public class Main {
    public static void main(String[] args) {
        Payment payment = PaymentFactory.createPayment("WeChat");
        payment.pay(); // 输出：WeChat Payment
    }
}
```

**(4) 类图：**
![export (8)](E:\hOutputSources\TyporaNote\面试\13、 设计模式.assets\export (8).svg)

**(5) 共性论：**
 与**单例模式**结合可确保全局唯一实例；与**抽象工厂模式**配合实现多级分类产品。

**(6) 利弊论与边界论：**

- **利：** 提高代码灵活性，符合开闭原则。
- **弊：** 工厂逻辑易变复杂；对简单场景略显冗余。
- **边界：** 适用于类种类多、实例化逻辑复杂、需求频繁变化的场景。

### **策略模式 (Strategy Pattern)**

**(1) 目的：** 定义一系列算法，将每种算法封装成独立的策略类，允许动态选择算法，而不影响客户端逻辑。

**(2) 编码场景：**

- 替代 `if-else` 或 `switch` 判断，动态选择处理逻辑。
- 例如：电商系统中，促销活动（满减、折扣、返现）根据用户选择动态应用对应策略。

**(3) 代码实现：**

```java
// 抽象策略
interface PromotionStrategy {
    void execute();
}

// 具体策略
class DiscountStrategy implements PromotionStrategy {
    public void execute() {
        System.out.println("Applying Discount Promotion");
    }
}
class CashbackStrategy implements PromotionStrategy {
    public void execute() {
        System.out.println("Applying Cashback Promotion");
    }
}

// 策略上下文
class PromotionContext {
    private PromotionStrategy strategy;

    public PromotionContext(PromotionStrategy strategy) {
        this.strategy = strategy;
    }

    public void applyStrategy() {
        strategy.execute();
    }
}

// 调用代码
public class Main {
    public static void main(String[] args) {
        PromotionContext context = new PromotionContext(new DiscountStrategy());
        context.applyStrategy(); // 输出：Applying Discount Promotion
    }
}
```

**(4) 类图**：
 ![export (9)](E:\hOutputSources\TyporaNote\面试\13、 设计模式.assets\export (9).svg)

```
classDiagram
    class PromotionStrategy {
        <<interface>>
        +execute() : void
        <<description>> "定义所有促销策略需实现的执行方法，是具体促销策略的抽象。"
    }

    class DiscountStrategy {
        +execute() : void
        <<description>> "实现折扣促销策略，执行时输出折扣促销相关信息，如'Applying Discount Promotion: 8折优惠！'。"
    }

    class CashbackStrategy {
        +execute() : void
        <<description>> "实现返现促销策略，执行时输出返现促销相关信息，如'Applying Cashback Promotion: 每满100元返现20元！'。"
    }

    class PromotionContext {
        -strategy : PromotionStrategy
        +PromotionContext(strategy : PromotionStrategy)
        +applyStrategy() : void
        <<description>> "策略上下文类，持有具体促销策略实例，并提供方法执行该策略。"
    }

    class Main {
        <<description>> "客户端代码所在类，用于创建不同促销策略的上下文并执行相应策略。"
    }

    Main --> PromotionContext : creates and calls
    PromotionContext --> PromotionStrategy : holds
    DiscountStrategy..|> PromotionStrategy
    CashbackStrategy..|> PromotionStrategy
```

**(5) 共性论：**
 与**模板方法**相比，策略模式更注重算法动态切换；与**工厂模式**结合可以动态生成策略对象。

**(6) 利弊论与边界论：**

- **利：** 避免硬编码，符合开闭原则；便于复用策略类。
- **弊：** 策略类增多；客户端需明确选择策略。
- **边界：** 适用于多变算法逻辑，差异大但流程一致的场景。

------

### **模板方法 (Template Method Pattern)**

**(1) 目的：** 定义一个算法框架，将具体步骤延迟到子类实现，以复用公共逻辑并允许局部自定义。
 **(2) 编码场景：**

- 需在不同实现中保留相同算法流程但允许由子类定制局部逻辑的场景。
- 例如：数据处理流程中，不同格式（CSV、JSON、XML）文件的读取和解析逻辑。

**(3) 代码实现：**

```java
// 抽象模板
abstract class DataProcessor {
    // 模板方法
    public void process() {
        readData();
        processData();
        saveData();
    }

    protected abstract void readData(); // 由子类实现
    protected abstract void processData();
    protected void saveData() {
        System.out.println("Data saved to database");
    }
}

// 具体子类
class CsvProcessor extends DataProcessor {
    protected void readData() {
        System.out.println("Reading CSV data");
    }
    protected void processData() {
        System.out.println("Processing CSV data");
    }
}

// 调用代码
public class Main {
    public static void main(String[] args) {
        DataProcessor processor = new CsvProcessor();
        processor.process();
    }
}
```

**(4) 类图：**

![export (10)](E:\hOutputSources\TyporaNote\面试\13、 设计模式.assets\export (10).svg)

```
classDiagram
    class DataProcessor {
        <<abstract>>
        +process() : void
        #readData() : void
        #processData() : void
        +saveData() : void
        <<description>> "抽象模板类，定义了数据处理的模板方法process()，包含读取、处理和保存数据的步骤。其中读取和处理数据的方法由子类实现，保存数据方法有默认实现。"
    }

    class CsvProcessor {
        +readData() : void
        +processData() : void
        <<description>> "具体子类，继承自DataProcessor，实现了抽象的读取数据和处理数据的方法，用于处理CSV格式数据。"
    }

    Main --> DataProcessor : creates
    CsvProcessor..|> DataProcessor
```

**(5) 共性论：**
 与**策略模式**相比，模板方法更注重固定流程和局部实现差异；与**责任链模式**配合可实现动态链式模板。

**(6) 利弊论与边界论：**

- **利：** 提高代码复用性；固定框架易于维护。
- **弊：** 子类依赖父类实现，增加继承关系。
- **边界：** 适用于流程固定、部分逻辑差异明确的场景。

### **责任链模式 (Chain of Responsibility Pattern)**

**(1) 目的：** 通过将请求沿着一条链传递，使多个对象有机会处理请求，从而解耦请求发送者和接收者。

**(2) 编码场景：**

- 当有多个对象可处理同一请求，但具体处理者不确定，且需动态决定谁来处理时。
- 例如：审批系统中，订单需按层级（组长、经理、总监）审批，但审批层级由金额决定。

**(3) 代码实现：**

```java
// 抽象处理者
abstract class Approver {
    protected Approver nextApprover;

    public void setNextApprover(Approver nextApprover) {
        this.nextApprover = nextApprover;
    }

    public abstract void approveRequest(int amount);
}

// 具体处理者
class Manager extends Approver {
    public void approveRequest(int amount) {
        if (amount <= 1000) {
            System.out.println("Manager approved the request");
        } else if (nextApprover != null) {
            nextApprover.approveRequest(amount);
        }
    }
}

class Director extends Approver {
    public void approveRequest(int amount) {
        if (amount <= 5000) {
            System.out.println("Director approved the request");
        } else if (nextApprover != null) {
            nextApprover.approveRequest(amount);
        }
    }
}

// 调用代码
public class Main {
    public static void main(String[] args) {
        Manager manager = new Manager();
        Director director = new Director();

        manager.setNextApprover(director);

        manager.approveRequest(3000); // 输出：Director approved the request
    }
}
```

**(4) 类图：**
![export (12)](E:\hOutputSources\TyporaNote\面试\13、 设计模式.assets\export (12).svg)

```
classDiagram
    class Approver {
        <<abstract>>
        -nextApprover : Approver
        +setNextApprover(nextApprover : Approver) : void
        +approveRequest(amount : int) : void
        <<description>> "抽象处理者类，定义了设置下一个处理者的方法以及审批请求的抽象方法，是具体处理者类的抽象基类。"
    }

    class Manager {
        +approveRequest(amount : int) : void
        <<description>> "具体处理者类，继承自Approver，负责审批金额不超过1000的请求，若金额超过则传递给下一个处理者。"
    }

    class Director {
        +approveRequest(amount : int) : void
        <<description>> "具体处理者类，继承自Approver，负责审批金额不超过5000的请求，若金额超过则传递给下一个处理者。"
    }

    Main --> Manager : creates
    Manager --> Director : sets next approver
    Manager..|> Approver
    Director..|> Approver
```

**(5) 共性论：**
 与**装饰器模式**相比，责任链模式更注重传递请求，而非增强功能；与**观察者模式**相比，责任链模式只有一个最终处理者，而观察者模式会通知所有观察者。

**(6) 利弊论与边界论：**

- **利：** 动态灵活地组合处理者；符合开闭原则，新增处理者只需扩展类并插入链条。
- **弊：** 可能造成链条过长，影响性能；不易排查问题源。
- **边界：** 适用于多个对象依次处理请求、且处理逻辑有顺序的场景。

### **建筑者模式 (Builder Pattern)**

**(1) 目的：** 分步构建复杂对象，允许按需灵活配置其组成部分，同时屏蔽细节实现。

**(2) 编码场景：**

- 需灵活构造对象，且对象包含多个可选或必选属性时。
- 例如：创建包含标题、内容、页脚的报告，不同场景需组合不同的属性。

**(3) 代码实现：**

```java
// 产品类
class Report {
    private String title;
    private String content;
    private String footer;

    public Report(String title, String content, String footer) {
        this.title = title;
        this.content = content;
        this.footer = footer;
    }

    @Override
    public String toString() {
        return "Title: " + title + ", Content: " + content + ", Footer: " + footer;
    }
}

// 建造者类
class ReportBuilder {
    private String title;
    private String content;
    private String footer;

    public ReportBuilder setTitle(String title) {
        this.title = title;
        return this;
    }

    public ReportBuilder setContent(String content) {
        this.content = content;
        return this;
    }

    public ReportBuilder setFooter(String footer) {
        this.footer = footer;
        return this;
    }

    public Report build() {
        return new Report(title, content, footer);
    }
}

// 调用代码
public class Main {
    public static void main(String[] args) {
        Report report = new ReportBuilder()
                            .setTitle("Annual Report")
                            .setContent("Financial Overview")
                            .setFooter("Confidential")
                            .build();
        System.out.println(report); // 输出完整的报告内容
    }
}
```

**(4) 类图：**
 略

**(5) 共性论：**
 与**工厂模式**相比，建筑者模式更适合复杂对象；与**原型模式**结合可用于复制并修改已有对象。

**(6) 利弊论与边界论：**

- **利：** 清晰分步构建，易于阅读与扩展；避免重载构造器的复杂性。
- **弊：** 对简单对象略显繁琐；需要额外的建造者类。
- **边界：** 适用于复杂对象构建、配置多且灵活变化的场景。

### **享元模式 (Flyweight Pattern)**

**(1) 目的：** 通过共享技术减少大量相似对象的内存占用，优化性能。

**(2) 编码场景：**

- 对象数量庞大、但大部分状态重复时，通过共享重复部分减少内存。
- 例如：绘图程序中绘制重复的树对象，每棵树共享其外观属性（如颜色、形状）。

**(3) 代码实现：**

```java
// 享元对象
class TreeType {
    private String color;
    private String shape;

    public TreeType(String color, String shape) {
        this.color = color;
        this.shape = shape;
    }

    public void draw(int x, int y) {
        System.out.println("Drawing " + color + " " + shape + " at (" + x + "," + y + ")");
    }
}

// 享元工厂
class TreeFactory {
    private static final Map<String, TreeType> treeTypes = new HashMap<>();

    public static TreeType getTreeType(String color, String shape) {
        String key = color + "-" + shape;
        if (!treeTypes.containsKey(key)) {
            treeTypes.put(key, new TreeType(color, shape));
        }
        return treeTypes.get(key);
    }
}

// 调用代码
public class Main {
    public static void main(String[] args) {
        TreeType tree1 = TreeFactory.getTreeType("Green", "Oak");
        tree1.draw(10, 20);

        TreeType tree2 = TreeFactory.getTreeType("Green", "Oak");
        tree2.draw(30, 40);
    }
}
```

**(4) 类图：**

![export (13)](E:\hOutputSources\TyporaNote\面试\13、 设计模式.assets\export (13).svg)

```
classDiagram
    class TreeType {
        -color : String
        -shape : String
        +TreeType(color : String, shape : String)
        +draw(x : int, y : int) : void
        <<description>> "享元对象类，代表树的类型，包含颜色和形状属性，通过draw方法可在指定坐标绘制该类型的树。"
    }

    class TreeFactory {
        -treeTypes : Map<String, TreeType>
        +getTreeType(color : String, shape : String) : TreeType
        <<description>> "享元工厂类，用于创建和管理享元对象（TreeType），通过维护一个Map来存储已创建的TreeType对象，根据传入的颜色和形状获取或创建对应的TreeType对象。"
    }

    Main --> TreeFactory : calls getTreeType
    TreeFactory --> TreeType : creates or retrieves
```

**(5) 共性论：**
 与**单例模式**相比，享元模式允许多个共享对象；与**代理模式**结合时，可延迟共享对象的创建。

**(6) 利弊论与边界论：**

- **利：** 大幅减少内存占用；共享对象可统一管理。
- **弊：** 共享对象状态难以扩展；非共享部分仍需独立管理。
- **边界：** 适用于状态多重复、对象数极大的场景。

### **观察者模式 (Observer Pattern)**

**(1) 目的：** 定义对象间一种一对多的依赖关系，当一个对象状态发生改变时，其所有依赖者都会收到通知并自动更新。

**(2) 编码场景：**

- 系统中对象之间存在触发关系，需确保相关对象能自动响应变化。
- 例如：新闻发布系统中，发布新闻后，订阅者会自动接收更新。

**(3) 代码实现：**

```java
// 观察者接口
interface Observer {
    void update(String message);
}

// 具体观察者
class Subscriber implements Observer {
    private String name;

    public Subscriber(String name) {
        this.name = name;
    }

    @Override
    public void update(String message) {
        System.out.println(name + " received update: " + message);
    }
}

// 被观察者接口
interface Subject {
    void addObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers(String message);
}

// 具体被观察者
class NewsPublisher implements Subject {
    private List<Observer> observers = new ArrayList<>();

    @Override
    public void addObserver(Observer o) {
        observers.add(o);
    }

    @Override
    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    @Override
    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }
}

// 调用代码
public class Main {
    public static void main(String[] args) {
        NewsPublisher publisher = new NewsPublisher();

        Observer subscriber1 = new Subscriber("Alice");
        Observer subscriber2 = new Subscriber("Bob");

        publisher.addObserver(subscriber1);
        publisher.addObserver(subscriber2);

        publisher.notifyObservers("Breaking news: Observer Pattern in action!");
    }
}
```

**(4) 类图：**

![export (14)](E:\hOutputSources\TyporaNote\面试\13、 设计模式.assets\export (14).svg)

```
classDiagram
    class Observer {
        <<interface>>
        +update(message : String) : void
        <<description>> "观察者接口，定义了更新方法，当被观察者状态改变并通知时，具体观察者通过此方法接收并处理更新信息。"
    }

    class Subscriber {
        -name : String
        +Subscriber(name : String)
        +update(message : String) : void
        <<description>> "具体观察者类，实现了Observer接口，持有观察者的名称属性，在update方法中根据接收到的消息输出相应的提示信息，表明该观察者已收到更新。"
    }

    class Subject {
        <<interface>>
        +addObserver(o : Observer) : void
        +removeObserver(o : Observer) : void
        +notifyObservers(message : String) : void
        <<description>> "被观察者接口，定义了添加、删除观察者以及通知所有观察者的方法，用于管理观察者列表并在状态变化时通知观察者。"
    }

    class NewsPublisher {
        -observers : List<Observer>
        +addObserver(o : Observer) : void
        +removeObserver(o : Observer) : void
        +notifyObservers(message : String) : void
        <<description>> "具体被观察者类，实现了Subject接口，内部维护一个观察者列表，通过实现接口中的方法来管理观察者，并在通知观察者时遍历列表调用每个观察者的update方法。"
    }

    Main --> NewsPublisher : creates and calls
    NewsPublisher --> Observer : manages
    Subscriber..|> Observer
    NewsPublisher..|> Subject
```

**(5) 共性论：**

- 与**责任链模式**相比，观察者模式会通知所有订阅者，而责任链模式只有一个最终处理者。
- 与**发布-订阅模式**相比，观察者模式更简单，直接依赖于对象间关系；发布-订阅模式常依赖消息队列实现。

**(6) 利弊论与边界论：**

- **利：** 解耦观察者与被观察者；支持动态增加观察者。
- **弊：** 当观察者过多时，通知可能耗费时间；观察者更新逻辑可能影响性能。
- **边界：** 适用于一对多依赖关系，且对象间需解耦的场景。

### **多例模式 (Multiton Pattern)**

**(1) 目的：** 确保一个类仅有有限数量的实例，且这些实例能被系统全局共享和访问。

**(2) 编码场景：**

- 当需要在系统中限制类实例的数量，并能通过标识符访问这些实例时。
- 例如：数据库连接池中，每个数据库对应一个连接实例。

**(3) 代码实现：**

```java
// 多例模式实现
class DatabaseConnection {
    private static final Map<String, DatabaseConnection> instances = new HashMap<>();
    private String dbName;

    private DatabaseConnection(String dbName) {
        this.dbName = dbName;
    }

    public static synchronized DatabaseConnection getInstance(String dbName) {
        if (!instances.containsKey(dbName)) {
            instances.put(dbName, new DatabaseConnection(dbName));
        }
        return instances.get(dbName);
    }

    public String getDbName() {
        return dbName;
    }
}

// 调用代码
public class Main {
    public static void main(String[] args) {
        DatabaseConnection db1 = DatabaseConnection.getInstance("db1");
        DatabaseConnection db2 = DatabaseConnection.getInstance("db2");
        DatabaseConnection db1Again = DatabaseConnection.getInstance("db1");

        System.out.println(db1.getDbName()); // 输出：db1
        System.out.println(db1 == db1Again); // 输出：true
        System.out.println(db1 == db2); // 输出：false
    }
}
```

**(4) 类图：**
 如数据库连接池，`getInstance` 方法通过标识符确保每个数据库实例唯一，同时实现共享访问。

**(5) 共性论：**

- 与**单例模式**相比，多例模式支持多个实例，每个实例与一个特定标识符绑定。
- 与**享元模式**相比，多例模式更注重通过标识符管理有限实例，而享元模式更关注内存优化和共享状态。

**(6) 利弊论与边界论：**

- **利：** 控制实例数量；支持实例全局访问。
- **弊：** 需要管理实例的生命周期；可能导致系统复杂性增加。
- **边界：** 适用于实例有限且需共享访问的场景，如资源池管理。

## 第三篇：最佳实践

### 一、不同的类型的消息 

```java
public abstract class AbstractMsgHandler<Req> {
    @Autowired
    private MessageDao messageDao;
    private Class<Req> bodyClass;

    @PostConstruct
    private void init() {
        ParameterizedType genericSuperclass = (ParameterizedType) this.getClass().getGenericSuperclass();
        this.bodyClass = (Class<Req>) genericSuperclass.getActualTypeArguments()[0];
        MsgHandlerFactory.register(getMsgTypeEnum().getType(), this);
    }

    /**
     * 消息类型
     */
    abstract MessageTypeEnum getMsgTypeEnum();

    protected void checkMsg(Req body, Long roomId, Long uid) {

    }

    @Transactional
    public Long checkAndSaveMsg(ChatMessageReq request, Long uid) {
        Req body = this.toBean(request.getBody());
        //统一校验
        AssertUtil.allCheckValidateThrow(body);
        //子类扩展校验
        checkMsg(body, request.getRoomId(), uid);
        Message insert = MessageAdapter.buildMsgSave(request, uid);
        //统一保存
        messageDao.save(insert);
        //子类扩展保存
        saveMsg(insert, body);
        return insert.getId();
    }

    private Req toBean(Object body) {
        if (bodyClass.isAssignableFrom(body.getClass())) {
            return (Req) body;
        }
        return BeanUtil.toBean(body, bodyClass);
    }

    protected abstract void saveMsg(Message message, Req body);

    /**
     * 展示消息
     */
    public abstract Object showMsg(Message msg);

    /**
     * 被回复时——展示的消息
     */
    public abstract Object showReplyMsg(Message msg);

    /**
     * 会话列表——展示的消息
     */
    public abstract String showContactMsg(Message msg);

}
public class MsgHandlerFactory {
    private static final Map<Integer, AbstractMsgHandler> STRATEGY_MAP = new HashMap<>();

    public static void register(Integer code, AbstractMsgHandler strategy) {
        STRATEGY_MAP.put(code, strategy);
    }

    public static AbstractMsgHandler getStrategyNoNull(Integer code) {
        AbstractMsgHandler strategy = STRATEGY_MAP.get(code);
        AssertUtil.isNotEmpty(strategy, CommonErrorEnum.PARAM_VALID);
        return strategy;
    }
}
@Component
public class TextMsgHandler extends AbstractMsgHandler<TextMsgReq> {
    @Autowired
    private MessageDao messageDao;
    @Autowired
    private MsgCache msgCache;
    @Autowired
    private UserCache userCache;
    @Autowired
    private UserInfoCache userInfoCache;
    @Autowired
    private IRoleService iRoleService;
    @Autowired
    private SensitiveWordBs sensitiveWordBs;

    private static final PrioritizedUrlDiscover URL_TITLE_DISCOVER = new PrioritizedUrlDiscover();

    @Override
    MessageTypeEnum getMsgTypeEnum() {
        return MessageTypeEnum.TEXT;
    }

    @Override
    protected void checkMsg(TextMsgReq body, Long roomId, Long uid) {
        //校验下回复消息
        if (Objects.nonNull(body.getReplyMsgId())) {
            Message replyMsg = messageDao.getById(body.getReplyMsgId());
            AssertUtil.isNotEmpty(replyMsg, "回复消息不存在");
            AssertUtil.equal(replyMsg.getRoomId(), roomId, "只能回复相同会话内的消息");
        }
        if (CollectionUtil.isNotEmpty(body.getAtUidList())) {
            //前端传入的@用户列表可能会重复，需要去重
            List<Long> atUidList = body.getAtUidList().stream().distinct().collect(Collectors.toList());
            Map<Long, User> batch = userInfoCache.getBatch(atUidList);
            //如果@用户不存在，userInfoCache 返回的map中依然存在该key，但是value为null，需要过滤掉再校验
            long batchCount = batch.values().stream().filter(Objects::nonNull).count();
            AssertUtil.equal((long)atUidList.size(), batchCount, "@用户不存在");
            boolean atAll = body.getAtUidList().contains(0L);
            if (atAll) {
                AssertUtil.isTrue(iRoleService.hasPower(uid, RoleEnum.CHAT_MANAGER), "没有权限");
            }
        }
    }

    @Override
    public void saveMsg(Message msg, TextMsgReq body) {//插入文本内容
        MessageExtra extra = Optional.ofNullable(msg.getExtra()).orElse(new MessageExtra());
        Message update = new Message();
        update.setId(msg.getId());
        update.setContent(sensitiveWordBs.filter(body.getContent()));
        update.setExtra(extra);
        //如果有回复消息
        if (Objects.nonNull(body.getReplyMsgId())) {
            Integer gapCount = messageDao.getGapCount(msg.getRoomId(), body.getReplyMsgId(), msg.getId());
            update.setGapCount(gapCount);
            update.setReplyMsgId(body.getReplyMsgId());

        }
        //判断消息url跳转
        Map<String, UrlInfo> urlContentMap = URL_TITLE_DISCOVER.getUrlContentMap(body.getContent());
        extra.setUrlContentMap(urlContentMap);
        //艾特功能
        if (CollectionUtil.isNotEmpty(body.getAtUidList())) {
            extra.setAtUidList(body.getAtUidList());

        }

        messageDao.updateById(update);
    }

    @Override
    public Object showMsg(Message msg) {
        TextMsgResp resp = new TextMsgResp();
        resp.setContent(msg.getContent());
        resp.setUrlContentMap(Optional.ofNullable(msg.getExtra()).map(MessageExtra::getUrlContentMap).orElse(null));
        resp.setAtUidList(Optional.ofNullable(msg.getExtra()).map(MessageExtra::getAtUidList).orElse(null));
        //回复消息
        Optional<Message> reply = Optional.ofNullable(msg.getReplyMsgId())
                .map(msgCache::getMsg)
                .filter(a -> Objects.equals(a.getStatus(), MessageStatusEnum.NORMAL.getStatus()));
        if (reply.isPresent()) {
            Message replyMessage = reply.get();
            TextMsgResp.ReplyMsg replyMsgVO = new TextMsgResp.ReplyMsg();
            replyMsgVO.setId(replyMessage.getId());
            replyMsgVO.setUid(replyMessage.getFromUid());
            replyMsgVO.setType(replyMessage.getType());
            replyMsgVO.setBody(MsgHandlerFactory.getStrategyNoNull(replyMessage.getType()).showReplyMsg(replyMessage));
            User replyUser = userCache.getUserInfo(replyMessage.getFromUid());
            replyMsgVO.setUsername(replyUser.getName());
            replyMsgVO.setCanCallback(YesOrNoEnum.toStatus(Objects.nonNull(msg.getGapCount()) && msg.getGapCount() <= MessageAdapter.CAN_CALLBACK_GAP_COUNT));
            replyMsgVO.setGapCount(msg.getGapCount());
            resp.setReply(replyMsgVO);
        }
        return resp;
    }

    @Override
    public Object showReplyMsg(Message msg) {
        return msg.getContent();
    }

    @Override
    public String showContactMsg(Message msg) {
        return msg.getContent();
    }
}

@Component
public class VideoMsgHandler extends AbstractMsgHandler<VideoMsgDTO> {
    @Autowired
    private MessageDao messageDao;

    @Override
    MessageTypeEnum getMsgTypeEnum() {
        return MessageTypeEnum.VIDEO;
    }

    @Override
    public void saveMsg(Message msg, VideoMsgDTO body) {
        MessageExtra extra = Optional.ofNullable(msg.getExtra()).orElse(new MessageExtra());
        Message update = new Message();
        update.setId(msg.getId());
        update.setExtra(extra);
        extra.setVideoMsgDTO(body);
        messageDao.updateById(update);
    }

    @Override
    public Object showMsg(Message msg) {
        return msg.getExtra().getVideoMsgDTO();
    }

    @Override
    public Object showReplyMsg(Message msg) {
        return "视频";
    }

    @Override
    public String showContactMsg(Message msg) {
        return "[视频]";
    }
}

@Component
public class ImgMsgHandler extends AbstractMsgHandler<ImgMsgDTO> {
    @Autowired
    private MessageDao messageDao;

    @Override
    MessageTypeEnum getMsgTypeEnum() {
        return MessageTypeEnum.IMG;
    }

    @Override
    public void saveMsg(Message msg, ImgMsgDTO body) {
        MessageExtra extra = Optional.ofNullable(msg.getExtra()).orElse(new MessageExtra());
        Message update = new Message();
        update.setId(msg.getId());
        update.setExtra(extra);
        extra.setImgMsgDTO(body);
        messageDao.updateById(update);
    }

    @Override
    public Object showMsg(Message msg) {
        return msg.getExtra().getImgMsgDTO();
    }

    @Override
    public Object showReplyMsg(Message msg) {
        return "图片";
    }

    @Override
    public String showContactMsg(Message msg) {
        return "[图片]";
    }
}

```

![export (15)](E:\aInputSources\技术文章\export (15).svg)

#### （一）设计模式分析

这段代码主要使用了五种设计模式。主要的设计模式包括：

1. 模板方法模式：通过 AbstractMsgHandler 定义消息处理的框架
2. 策略模式：使用不同的消息处理器处理不同类型的消息
3. 工厂模式：通过 MsgHandlerFactory 管理和创建消息处理器
4. 泛型模式：使用泛型提供类型安全性
5. 单例模式：通过 Spring 的依赖注入确保处理器的单例性

##### 1. 模板方法模式 

`AbstractMsgHandler` 抽象类中使用了模板方法模式，它定义了消息处理的骨架流程：

- 抽象类定义了算法的骨架：

  ```java
  public Long checkAndSaveMsg(ChatMessageReq request, Long uid) {
      Req body = this.toBean(request.getBody());
      AssertUtil.allCheckValidateThrow(body);
      checkMsg(body, request.getRoomId(), uid);
      Message insert = MessageAdapter.buildMsgSave(request, uid);
      messageDao.save(insert);
      saveMsg(insert, body);
      return insert.getId();
  }
  ```

- 子类必须实现的抽象方法：

  - `abstract MessageTypeEnum getMsgTypeEnum()`
  - `protected abstract void saveMsg(Message message, Req body)`
  - `public abstract Object showMsg(Message msg)`
  - `public abstract Object showReplyMsg(Message msg)`
  - `public abstract String showContactMsg(Message msg)`

##### 2. 策略模式 

通过 `MsgHandlerFactory` 和不同的消息处理器实现了策略模式：

- `TextMsgHandler`、`VideoMsgHandler`、`ImgMsgHandler` 都是具体的策略实现

- `MsgHandlerFactory` 用于管理和选择具体的策略：

  ```java
  public static AbstractMsgHandler getStrategyNoNull(Integer code) {
      AbstractMsgHandler strategy = STRATEGY_MAP.get(code);
      AssertUtil.isNotEmpty(strategy, CommonErrorEnum.PARAM_VALID);
      return strategy;
  }
  ```

##### 3. 工厂模式 

`MsgHandlerFactory` 实现了简单工厂模式：

- 通过 `register` 方法注册不同类型的消息处理器
- 通过 `getStrategyNoNull` 方法获取具体的处理器实例
- 使用 Map 存储不同类型的处理器实例

##### 4. 泛型模式

使用了泛型来增强类型安全性和代码复用：

```java
public abstract class AbstractMsgHandler<Req> {
    private Class<Req> bodyClass;
    
    private Req toBean(Object body) {
        if (bodyClass.isAssignableFrom(body.getClass())) {
            return (Req) body;
        }
        return BeanUtil.toBean(body, bodyClass);
    }
}
```

##### 5. 单例模式 

通过 Spring 的 `@Component` 注解，每个具体的消息处理器都是单例的：

- `TextMsgHandler`
- `VideoMsgHandler`
- `ImgMsgHandler`

#### （二）设计模式的优点

1. **可扩展性**：
   - 新增消息类型只需添加新的处理器类
   - 不需要修改现有代码，符合开闭原则

2. **代码复用**：
   - 通过抽象类复用通用逻辑
   - 减少重复代码

3. **维护性**：
   - 各个消息类型的处理逻辑独立
   - 职责明确，易于维护和测试

4. **类型安全**：
   - 使用泛型确保类型安全（泛型的一个重要作用，诸如List、HashMap都有所体现）
   - 编译时即可发现类型错误

#### （三）改进：责任链模式 

##### 1. **分析上面的代码**

- 消息的校验 (`checkMsg`) 和保存 (`saveMsg`) 逻辑可以被视为一种责任链，多个步骤（通用校验、子类扩展校验、消息保存、附加信息保存）以层层调用的形式实现。

在当前代码中，我们需要对消息进行校验、保存、额外处理（例如敏感词过滤或记录日志）。如果未来需求变化，比如新增或调整处理流程，现有代码会遇到什么问题？

- 校验和保存逻辑耦合在一起，不容易拆分和扩展。
- 增加功能时会在现有代码中插入新的逻辑，导致代码膨胀。
- 随着需求复杂化，代码的阅读和维护难度会大幅提升。

这正是我们要解决的问题。当前的代码结构就像修建在沙滩上的房子，短期内能住人，但无法应对风暴。我们需要一套更灵活、更模块化的架构，而责任链模式正是为此设计的。

##### 2. **责任链模式的核心思想**

责任链模式的核心思想是**分离职责，链式传递**。每个处理步骤都由独立的节点实现，这些节点通过链表串联，依次处理请求。你可以将它类比为一个生产线，每个工位专注于一个功能。如果新增或调整某个工位，只需改变链条，而不会影响其他部分。

**定义**：将多个对象串联成一条链，当请求到来时，链中的对象依次处理，直到某个对象能够完全处理该请求。

想象一下，一条消息生产线上：

- 第一个工位检查原材料质量（校验消息）。
- 第二个工位进行加工（保存消息）。
- 第三个工位负责后续处理（例如敏感词过滤）。

如果有一天需要在加工前增加清洗步骤（敏感词过滤），责任链模式只需在链条中插入一个“清洗工位”，而不用大改原有逻辑。

##### 3. **用责任链模式改造现有逻辑**

在责任链模式中，如何定义每个处理节点？它们之间如何协作？

- 每个节点可以实现一个通用接口，例如 `MsgHandlerNode`。
- 通过链表或数组维护节点顺序。
- 每个节点处理后，决定是否将请求传递给下一个节点。

接下来，我们用代码实现这个逻辑：

```java
public interface MsgHandlerNode {
    boolean handle(Message message);
}

public class CheckMessageNode implements MsgHandlerNode {
    @Override
    public boolean handle(Message message) {
        if (!message.isValid()) {
            System.out.println("校验失败！");
            return false;
        }
        System.out.println("校验通过！");
        return true;
    }
}

public class SaveMessageNode implements MsgHandlerNode {
    @Override
    public boolean handle(Message message) {
        System.out.println("保存消息：" + message.getContent());
        return true;
    }
}
```

通过 `MsgHandlerChain` 管理这些节点：

```java
public class MsgHandlerChain {
    private final List<MsgHandlerNode> nodes = new ArrayList<>();
    public MsgHandlerChain addNode(MsgHandlerNode node) {
        nodes.add(node);
        return this;
    }
    public void process(Message message) {
        for (MsgHandlerNode node : nodes) {
            if (!node.handle(message)) {
                System.out.println("处理被中断！");
                break;
            }
        }
    }
}
```

##### 4. **喝一口茶**

如果我们现在要新增一个日志记录功能，该如何添加？

- 定义一个新的 `LogMessageNode` 实现 `MsgHandlerNode` 接口。
- 将它插入到 `MsgHandlerChain` 中的合适位置。

##### 5. **再反思：适用场景与边界条件**

责任链模式适用于什么样的场景？它的边界在哪里？

- **适用场景**：需要按照固定流程处理的逻辑（如校验、权限检查、日志记录等）。
- **边界条件**：处理流程不能过于简单，否则责任链会增加不必要的复杂性。

**需要注意的是**：在实际项目中，如果只有两三步简单逻辑，不需要引入责任链模式。责任链的价值在于复杂流程的管理和扩展。

#### （四）改进后的代码

所以就目前来说，我们并不需要改进这段代码，但是出于学习目的，我们还是进行了代码的重构。

为了将当前代码架构转换为责任链模式，我们需要明确以下几个要点：

- **责任链的核心思想**：
   将多个处理步骤串联起来，当一个请求进入时，由链上的节点逐步处理，直到请求被完全处理或链结束。

- **改造目标**：
   使校验 (`checkMsg`) 和保存 (`saveMsg`) 等逻辑成为链条上的独立节点，每个节点完成自己的职责并决定是否传递给下一个节点。

##### 1. 定义责任链节点接口

每个节点负责一个处理步骤，通过实现以下接口加入链中：

```java
public interface MsgHandlerNode {
    boolean handle(Message message);
}
```

##### 2. 实现校验和保存节点

将 `checkMsg` 和 `saveMsg` 等逻辑拆分为单独的节点：

```java
// 校验节点
public class CheckMessageNode implements MsgHandlerNode {
    @Override
    public boolean handle(Message message) {
        // 校验逻辑
        if (!message.isValid()) {
            System.out.println("Message validation failed.");
            return false;
        }
        System.out.println("Message validation passed.");
        return true; // 继续传递
    }
}

// 保存节点
public class SaveMessageNode implements MsgHandlerNode {
    @Override
    public boolean handle(Message message) {
        // 保存逻辑
        System.out.println("Message saved: " + message.getContent());
        return true; // 继续传递
    }
}

// 扩展：附加功能节点
public class AdditionalProcessingNode implements MsgHandlerNode {
    @Override
    public boolean handle(Message message) {
        // 执行附加逻辑
        System.out.println("Additional processing completed.");
        return true; // 继续传递
    }
}
```

##### 3. 构建责任链

通过链式调用组织节点的执行顺序：

```java
public class MsgHandlerChain {
    private final List<MsgHandlerNode> nodes = new ArrayList<>();

    public MsgHandlerChain addNode(MsgHandlerNode node) {
        nodes.add(node);
        return this;
    }

    public void process(Message message) {
        for (MsgHandlerNode node : nodes) {
            if (!node.handle(message)) {
                System.out.println("Processing stopped at: " + node.getClass().getSimpleName());
                break;
            }
        }
    }
}
```

##### 4. 客户端调用示例

通过链式结构调用处理逻辑：

```java
public class Main {
    public static void main(String[] args) {
        // 构建责任链
        MsgHandlerChain chain = new MsgHandlerChain()
            .addNode(new CheckMessageNode())
            .addNode(new SaveMessageNode())
            .addNode(new AdditionalProcessingNode());

        // 模拟消息处理
        Message message = new Message("Hello, Chain of Responsibility!");
        chain.process(message);
    }
}
```

##### 5. 示例运行结果

假设输入消息 `Hello, Chain of Responsibility!`，输出可能如下：

```
Message validation passed.
Message saved: Hello, Chain of Responsibility!
Additional processing completed.
```

如需进一步优化或扩展，可以结合 **责任链模式与工厂模式** 生成链条，动态配置链的节点顺序。

### 二、单例模式

单例模式（Singleton Pattern）是一种常用的软件设计模式，用于确保一个类仅有一个实例，并提供一个全局访问点。以下是使用Java语言实现单例模式的几种常见方式：

##### 1. 懒汉式（线程不安全）

```java
public class Singleton {  
    private static Singleton instance;  
  
    private Singleton() {}  
  
    public static Singleton getInstance() {  
        if (instance == null) {  
            instance = new Singleton();  
        }  
        return instance;  
    }  
}
```

**注意**：这种方式在多线程环境下是不安全的，可能会创建多个实例。

##### 2. 懒汉式（线程安全）

```java
public class Singleton {  
    private static Singleton instance;  
  
    private Singleton() {}  
  
    public static synchronized Singleton getInstance() {  
        if (instance == null) {  
            instance = new Singleton();  
        }  
        return instance;  
    }  
}
```

**注意**：虽然这种方式是线程安全的，但每次调用`getInstance()`方法时都会进行同步，这会影响性能。

##### 3. 双重检查锁定（Double-Checked Locking）

```java
public class Singleton {  
    private static volatile Singleton instance;  
  
    private Singleton() {}  
  
    public static Singleton getInstance() {  
        if (instance == null) {  
            synchronized (Singleton.class) {  
                if (instance == null) {  
                    instance = new Singleton();  
                }  
            }  
        }  
        return instance;  
    }  
}
```

**注意**：这种方式既保证了线程安全，又避免了每次调用`getInstance()`方法时都进行同步，提高了性能。`volatile`关键字确保了多线程环境下`instance`变量的可见性和禁止指令重排序。

##### 4. 静态内部类

```java
public class Singleton {  
    private Singleton() {}  
  
    private static class SingletonHolder {  
        private static final Singleton INSTANCE = new Singleton();  
    }  
  
    public static final Singleton getInstance() {  
        return SingletonHolder.INSTANCE;  
    }  
}
```

**注意**：这种方式利用了classloder的机制来保证初始化`Singleton`类时只有一个线程能创建`SingletonHolder`类，从而保证了`INSTANCE`的唯一性。同时，这种方式也实现了懒加载。

##### 5. 枚举

```java
public enum Singleton {  
    INSTANCE;  
  
    public void whateverMethod() {  
    }  
}
```

**注意**：这是实现单例模式的最佳方法。它更简洁，自动支持序列化机制，绝对防止多次实例化，即使在面对复杂的序列化或者反射攻击的时候，这种单例的类型仍然可以安全地保证只有一个实例。



![img](E:\hOutputSources\TyporaNote\面试\13、 设计模式.assets\oli9z0imbc.gif)

### 三、利用Redis+策略模式实现限流

![export (7)](E:\hOutputSources\TyporaNote\面试\13、 设计模式.assets\export (7).svg)

### 番外、如何选用适用的设计模式

#### (零) 基础：指令.过程.对象（TODO）

我们从高级语言、中级语言、低级语言中各自取出一种，进行讨论：

- **Java语言的定义**：Java 是一种完全**面向对象**的**高级编程语言**。
- **C语言的定义**：C 语言是一种通用的、**面向过程**的高级编程语言，同时也具有一些接近底层硬件的特性，介于高级语言和低级语言之间，常被看作是**中级语言**。
- **机器语言的定义**：机器语言是计算机能够直接识别和执行的二进制**指令**代码，是最底层的计算机语言。

机器语言由指令组成，关注点在于：

C语言是对机器语言的封装，关注点在于：分支控制（for/if）、函数、结构体、指针；

Java语言是对C语言的进一步发展，关注点在于对象与类，继承封装多态。

后者是对前者的封装和发展，前者是后者的基础。所以理论上，前者的形式化是后者，后者可还原为前者。

故而，我们作出如下理解：

（从中级语言到低级语言）一切程序都是指令，指令按顺序执行。单纯for()是同样的指令段重复执行，单纯if()是可以消失指令段。

（从高级语言到低级语言）类包含属性 函数和方法，类 是指令篇，对象是类的实现，具体来说，补充了类的属性，是指令篇。

没有继承封装和多态：高级语言回到中级语言。

封装：过程封装为类的方法，补充。

继承：指令段的复用，重复的指令段变为父类。

多态：补充。

![graph](E:\hOutputSources\TyporaNote\面试\13、 设计模式.assets\graph.svg)

```
digraph LanguageHierarchy {
    // 设置节点的通用样式，这里设置形状为矩形，字体大小等
    node [shape=box, fontsize=10];

    // 定义三个子图，分别对应高级语言区、中级语言区和低级语言区，用方框来直观表示区域
    subgraph cluster_high_level {
        label="高级语言区";
        bgcolor="lightblue";

        "Python" [label="Python"];
        "Java" [label="Java"];
        "JavaScript" [label="JavaScript"];
        "Ruby" [label="Ruby"];
        "PHP" [label="PHP"];

        // 可根据需要添加内部节点之间的连接关系（这里暂未添加，可自行根据实际需求补充）
        // "Python" -> "Java" [label="某种关联", style=dashed];
    }

    subgraph cluster_mid_level {
        label="中级语言区";
        bgcolor="pink";

        "C" [label="C"];
        "C++" [label="C++"];
        "Objective-C" [label="Objective-C"];
        "Swift" [label="Swift"];
        "Assembly" [label="Assembly"];

        // 同样可根据需要添加内部节点之间的连接关系（这里暂未添加，可自行根据实际需求补充）
        // "C" -> "C++" [label="某种关联", style=dashed];
    }

    subgraph cluster_low_level {
        label="低级语言区";
        bgcolor="lightgray";

        "Machine Language" [label="Machine Language"];
        "Microcode" [label="Microcode"];

        // 可根据需要添加内部节点之间的连接关系（这里暂未添加，可自行根据实际需求补充）
        // "Machine Language" -> "Microcode" [label="某种关联", style=dashed];
    }

    // 设置节点的布局方向为从左到右（'LR'）
    //attr(rankdir='LR');

   
}
```

#### (一) **语言层次与设计模式**（TODO）

在编程语言中，高级语言、中级语言和低级语言分别代表着不同的抽象层次。我们可以通过语言层次的演进来理解设计模式的背景与核心：

1. **低级语言：机器语言**
   - 关注点：**指令**
      机器语言是直接与硬件交互的二进制代码，所有操作以指令为单位执行。
2. **中级语言：C语言**
   - 关注点：**过程**
      C语言通过分支控制（如 `for`、`if`）、函数和指针等抽象特性，为指令的组织提供了灵活性和可复用性。
3. **高级语言：Java语言**
   - 关注点：**对象与类**
      Java进一步引入面向对象特性（如继承、封装、多态），将代码抽象到类与对象的层次，使得程序逻辑更具可读性和扩展性。

#### (二) **设计模式的分类与选择**

在实际开发中，设计模式帮助我们处理对象的创建、组织结构和行为分工的问题。以下对创建型、结构型和行为型模式进行分类说明：

##### **1. 创建型模式（Creational Patterns）**：关注**对象创建**的过程，解耦实例化代码。

- **工厂模式**

  - 背后思想：创建具体对象的逻辑应独立于使用者。

  - 场景：需要灵活地根据条件创建不同类型的对象。

  - 示例：通过抽象工厂生产不同的 GUI 组件（按钮、文本框）

- **单例模式**

  - 背后思想：限制类的实例化，确保全局唯一性。

  - 场景：需要全局共享状态，如配置管理器。

  - 示例：数据库连接池。

- **建造者模式**

  - 背后思想：将复杂对象的构建与表示分离。

  - 场景：构造具有多种配置选项的对象。

  - 示例：生成一个复杂的报表或对象配置器。

- **原型模式**

  - 背后思想：通过**复制**已有实例生成新对象，避免类的直接实例化。

  - 场景：对象创建开销大或具有复杂初始化逻辑。

  - 示例：克隆一个图形对象或用户配置。

##### **2. 结构型模式（Structural Patterns）**：关注**类与对象的组合**，解决类之间的耦合问题。

1. **适配器模式**
   - 背后思想：将不兼容的接口转为兼容的形式。
   - 场景：旧系统模块需要与新系统交互。
   - 示例：为第三方库提供兼容层。
2. **代理模式**
   - 背后思想：通过代理对象控制对目标对象的访问。
   - 场景：访问控制或性能优化（如懒加载、缓存）。
   - 示例：安全检查或日志记录。
3. **装饰器模式**
   - 背后思想：动态地为对象添加新功能，而无需修改其代码。
   - 场景：扩展现有功能而不破坏原有类的封装。
   - 示例：为文本框添加滚动条或背景色。
4. **享元模式**
   - 背后思想：通过共享技术避免对象创建的高成本。
   - 场景：需要大量相似对象实例。
   - 示例：字符渲染中的字形复用。

##### **3. 行为型模式（Behavioral Patterns）**：核心思想：关注**对象之间的职责分配**与协作方式。

- **责任链模式**

  - 背后思想：请求沿着链传递，找到合适的处理者。

  - 场景：审批流程、事件处理机制。

  - 示例：日志系统中按级别记录日志。

- **观察者模式**

  - 背后思想：实现一对多的依赖通知。

  - 场景：订阅发布系统。

  - 示例：消息队列的订阅机制。

- **策略模式**

  - 背后思想：将算法封装在类中，使得算法可以自由切换。

  - 场景：支付方式切换（如微信支付、支付宝）。

  - 示例：不同的排序算法。

- **模板方法模式**

  - 背后思想：定义算法骨架，延迟某些步骤到子类实现。

  - 场景：共享通用流程但细节实现不同。

  - 示例：文件读取器的模板逻辑。

#### (三)  **从问题到模式的选择**

##### **问：有类 A、B、C，三者有一方法逻辑相同，也有异处，以何兼共拓异？**

**答曰：** 构建一辅助抽象类 AB，共性逻辑写入其方法；A、B、C继承 AB，重写各自的方法以实现差异化。

##### **选择示例**

- 使用 **模板方法模式** 提取共性流程，留出可变部分。
- 使用 **策略模式** 分离算法逻辑，动态切换实现。

通过这样的设计，系统具备更高的灵活性与可扩展性。

#### (四) **总结：关键词驱动的模式应用**

设计模式可以通过三个关键词（**创建**、**结构**、**行为**）帮助开发者快速定位问题与方案：

1. 创建型模式解决对象实例化的复杂性。
2. 结构型模式优化对象与对象之间的协作。
3. 行为型模式理顺职责分配与动态交互。

关键词：

1. **对象创建管理：** 选择创建型模式。
2. **接口适配：** 选择适配器模式。
3. **动态功能扩展：** 选择装饰器模式。
4. **控制访问：** 选择代理模式。
5. **行为变化依赖状态：** 选择模板方法或策略模式。
6. **链式处理：** 选择责任链模式。
7. **一对多通知：** 选择观察者模式。

> **实践建议：** 在具体业务场景中，仔细分析需求的共性与差异性，从而选择合适的设计模式为系统设计赋能。r如：
>
> 需要对对象实例进行创建和存储和管理，涉及到的可能是创建型模式。
>
> 如果需要把一个对象实例变为另一个对象实例，涉及到的可能是适配器模式。
>
> 如果要把一个方法的前面或后面加上一些方法，涉及到的可能是代理模式或装饰器模式。
>
> 如果代码用到子类与父类的继承关系，并且子类之间是非近亲类，并且实现了一些方法，仅有父类调用，那么可能涉及到的到的是模板方法。
>
> 如果如果代码用到子类与父类的继承关系，并且子类之间是近亲类，调用者传入了一些非资源标识符（即功能标识符），那么可能涉及到的到的是策略模式。
>
> 如果一些类代表了不同职责，并且以链表形式组织在一起，外层循环传递责任，那么可能是责任链模式。责任链模式分为三种：分工合作责任链（没个节点都负责一部分），分工单干平级责任链（找到一个符合的，功业务非递进关系），分工单干非平级级责任链（找到一个符合的，功业务递进关系）。
>
> 如果一个类改变，需要对其他多个类也进行改变(list组合)，循环通知，可能是观察者模式。
>
> 还有一些可能是组合，策略模式+单例工厂（map），key取出策略。策略模式+多例工厂（new）。

## 第四篇：Spring 中设计模式（TODO）

Spring框架中广泛运用了多种设计模式，以实现其强大的功能和灵活性。以下是一些Spring框架中常见的设计模式：

1. 单例模式（Singleton Pattern）

   - 在Spring的默认作用域中，每个Bean都是单例的，即在整个Spring IoC容器中，每个Bean只会有一个实例。这有助于节省资源并提高性能。

2. 工厂模式（Factory Pattern）

   - Spring通过BeanFactory和ApplicationContext等接口创建并管理对象实例。这种方式将对象的创建与使用解耦，使得程序更加灵活和可扩展。

3. 代理模式（Proxy Pattern）

   - Spring的AOP（面向切面编程）功能大量使用了代理模式。AOP通过在目标方法执行前后添加额外的行为（如日志、事务管理等），而这些额外的行为是通过代理对象来实现的。Spring提供了两种代理方式：JDK动态代理和CGLIB代理。

4. 模板方法模式（Template Method Pattern）

   - 在Spring的JdbcTemplate、HibernateTemplate等类中，使用了模板方法模式。这些类定义了一个操作中的算法骨架，而将一些步骤延迟到子类中实现。这种方式使得Spring可以为不同的数据库操作提供统一的接口，同时允许用户根据自己的需求进行定制。

5. 观察者模式（Observer Pattern）

   - Spring的事件处理机制就是观察者模式的一个应用。当某个事件发生时，所有注册的观察者都会自动收到通知并作出相应的处理。这种方式使得事件的处理更加解耦和灵活。

6. 适配器模式（Adapter Pattern）

   - Spring MVC中的Controller适配器，以及AOP模块中的适配器，都使用了适配器模式。适配器模式用于将一个接口转换为客户希望的另一个接口，使得不同的类或对象能够协同工作。

7. 装饰器模式（Decorator Pattern）

   - 在Spring AOP中，代理对象（代理类）就是对目标对象的增强（装饰），可以动态地为目标对象添加新的行为（如方法拦截、日志记录、事务管理等）。

8. 策略模式（Strategy Pattern）

   - 在Spring中，策略模式主要用于实现不同的算法或策略。例如，Spring的TaskScheduler接口就定义了不同的任务调度策略，如同步执行、异步执行等。

9. 依赖注入（Dependency Injection, DI）和控制反转（Inversion of Control, IoC）

   - 这两个设计模式是Spring框架的核心。通过依赖注入，Spring负责管理和注入组件之间的依赖关系，降低了组件之间的耦合度。控制反转则是指将对象的创建和依赖关系的管理交给Spring容器，从而实现了对象的解耦。

此外，Spring框架还使用了其他设计模式，如组合模式、建造者模式、委托模式、空对象模式等，以实现各种功能和组件。这些设计模式的运用使得Spring框架更加灵活、可扩展和易于维护。

## 参考文献

有时间补上