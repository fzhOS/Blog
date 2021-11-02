#### new 运算符：

- 常规new 运算符：

  	- 在堆内存（heap）中分配空间。
  	- 动态内存由new和delete控制，而不是由作用域和链接性规则控制。因此，可以在一个函数中分配动态内存，在另一个函数中释放内存。
  	- new失败时，以前是让new返回空指针，现在是将引发一场std::bad_alloc。
  	- new和delete运算符分别调用分配函数和释放函数。

- 定位new运算符：

  - new运算符还有一种变体，被称为**定位**（placement）new运算符，他让你能够指定要使用的位置。

  - 要使用new定位特性，首先需要包含**头文件new**，它提供了这种版本的new运算符的原型，然后将new运算符用于提供可所需地址的参数。

    ```c++
    #include<new>
    struct chaff{
      char dross[20];
      int slag;
    };
    char buffer1[50];
    char buffer2[500];
    int main(int argc,char * argv[]){
      chaff *p1,*p2;
      int *p3,*p4;
      //the regular forms of new 
      p1=new chaff;   //place structure in heap(堆内存)
      p3=new int[20]; //place int array in heap（堆内存）
      //the two forms of placement new
      p2=new(buffer1) chaff;  //place structure in buffer1（静态内存）
      p4=new(buffer2) int[20];  //place int array in buffer2（静态内存）
     	...
      ...
      ...
      delete p1;  //释放堆内存
     	delete [] p3; //释放堆内存
      //思考为啥没有用delete 释放定位new运算符分配的内存？
      return 0;
    }
    ```

  - 这个实例使用两个静态数组来为定位new运算符提供内存空间，上述代码性buffer1中分配空间给结构chaff，从buffer2中分配空间给一个包含20个元素的int数组。

  - 上述代码中，buffer指定的内存是**静态内存**，而delete只能用于这样的指针：指向**常规**new运算符分配的堆内存。即 **数组buffer位于delete的管辖范围之外**。

  - 所以```delete [] p2; //won't work ```在运行阶段会失败。

  - 如果buffer是使用常规new运算符创建的，便可以使用常规delete运算符来释放整个内存块。

  - 定位new运算符的额工作原理：基本上，它只是返回传递给它的地址，并将其强制转化为void *,以便能够赋给任何指针类型。

  - 将new运算符用于类对象时，情况将更加复杂。

  - 就像常规new调用一个接受一个参数的new（）函数一样，标准定位new调用一个接受两个参数的new（）函数：

    ```c++
    int *p1=new int;  //invoke(调用) new(sizeof(int))
    int *p2=new(buffer) int; //invoke new(sizeof(int),buffer)
    int *p3=new(buffer) int[40];//invoke new(40*sizeof(int),buffer)
    ```

#### 名称空间：

- 

