### 通配符&泛型
**源码参考：**
java.util.Comparator#comparing(java.util.function.Function<? super T,? extends U>)


通配符 ? 和泛型在 Java 中都涉及到类型的灵活性和代码的复用性。下面是它们的区别和共同点以及为什么它们被设计以及它们的作用：

区别：

泛型是用于在声明类、接口或方法时指定类型参数，以增加类型安全性。它允许在编译时对参数或返回值进行类型检查和类型约束。通过使用泛型，可以在编译期间捕获类型错误，从而减少运行时错误和异常。
通配符 ? 是用于声明未知类型的占位符，它在使用类型时提供了更大的灵活性。它可以表示任意类型、某个特定类型的未知子类型或未知超类型。

共同点：

泛型和通配符都是用于增加代码的灵活性和复用性。
它们都允许在方法参数、字段、返回值等处使用不确定的类型。

为什么设计这两个：

泛型的设计是为了解决类型安全性和代码复用性的问题。它在**编译期间可以检查和约束类型**，从而减少类转换错误和类型错误。
通配符的设计是为了使代码能够适应多种类型的情况，而**不需要指定具体的类型**。使用通配符，可以编写更通用、灵活的代码，适用于不同类型的对象。

举例说明：

泛型的使用：
java
<button><svg><path></path></svg><span>Copy code</span><span></span></button>
public class Box<T> {
    private T item;

    public void setItem(T item) {
        this.item = item;
    }

    public T getItem() {
        return item;
    }
}

在这个例子中，T 是一个泛型参数，它表示 Box 类的类型参数。通过使用泛型，可以在编译时确保在传递或返回 Box 对象时使用的是正确的类型。

通配符的使用：
java
<button><svg><path></path></svg><span>Copy code</span><span></span></button>
public void printList(List<?> list) {
    for (Object item : list) {
        System.out.println(item);
    }
}

在这个例子中，List<?> 使用了通配符 ? 表示使用不确定的类型。这段代码允许传递任何类型的 List 对象，并打印出列表中的元素。这提供了更大的灵活性，使得该方法适用于不同类型的 List。
