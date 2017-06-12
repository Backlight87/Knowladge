### try catch finally相关知识点
1. 注意一点，并不是先执行try再执行catch最后执行finally ，顺序是执行try语句块的时候catch去监听有没有错误，有的话就跳到catch语句块执行完catch接着执行finally，如果try执行顺利没有错误，那就执行完try直接去执行finally。
2. finally语句块一定会执行么？
   理论上是一定会执行的，除了一下这两种情况：
   - 在try之前就执行了return，很明显这种情况下，整个try -catch- finally模块都不会执行
   - 在try-catch执行过程中执行了 ：``` system.exit(0) ```，这个语句会导致jvm直接停止了，当然一切程序都停止了，finally语句块也就执行不了了

3. try语句块里有return的情况下（这里默认都没有异常，即）
    - 第一种：举个例子：
    ```
    public static int test1() {
        int b = 20;

        try {
            System.out.println("try block");

            return b += 80;
        }
        catch (Exception e) {

            System.out.println("catch block");
        }
        finally {

            System.out.println("finally block");

            if (b > 25) {
                System.out.println("b>25, b = " + b);
            }
        }

        return b;
    }
    ```
    对于这种，他会先执行try然后执行``` b += 80 ```,然后执行finally的语句块，然后执行return。
    - 第二种，如果finally也有return语句，那么会执行到finally语句的return，而不会执行try里面的return，不过这时候try里面是不能写return的，因为这条return是不可达状态会报错的。

    
