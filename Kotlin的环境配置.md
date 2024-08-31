# Kotlin环境的安装和配置

1. 安装JDK环境 8以上即可

2. 前往Github下载：https://github.com/JetBrains/kotlin/releases Kotlin的安装 下载[kotlin-compiler-2.0.20.zip](https://github.com/JetBrains/kotlin/releases/download/v2.0.20/kotlin-compiler-2.0.20.zip)即可，有新版本可以直接下载新的，本笔记编写时候为这个版本捏 Ciallo～(∠・ω< )⌒☆

3. 把他解压到喜欢的位置，然后配置环境变量![image-20240831182603698](assets/image-20240831182603698.png)

   (注意：在这里踩坑了，如果要放到Program Files里面只能放在C盘的Program Files 如果你有和我一样的习惯，在D盘创建一个Program Files 安装kotlin会导致kotlin安装失败，引以为戒QAQ) 

4. 可以使用经典的kotlinc -version来验证下

   ```
   C:\Users\AFish>kotlinc -version
   info: kotlinc-jvm 2.0.20 (JRE 21+35-2513)
   ```

5. Hello World 程序

   ```kotlin
   fun main() {
       println("Hello, World!")
   }
   ```

   通过指令cmd进行编译，并执行 kotlinc main.kt -include-runtime -d main.jar

   ```
   C:\Users\AFish>cd Desktop
   
   C:\Users\AFish\Desktop> kotlinc main.kt -include-runtime -d main.jar
   
   C:\Users\AFish\Desktop>java -jar main.jar
   Hello, World!
   ```

   