+ 辅助理解递归：明确这个函数是做什么的，然后在这个函数里有需要做这一步，比如按扩展先序创建二叉树，函数作用为：“创建输入节点的子树，先左子树，后右子树”。然后创建左右子树的时候是递归调用。
+ 管道和进程间通信，即`pipe()`+`fork()`的形式，注意关掉无用的文件文件描述符，否则会造成文件描述符的浪费，并且fork()后关闭比较麻烦。先pipe()，之后先把**能立刻用完****之后就不在用的符**立刻用完，然后将他们关掉，再fork()。因为子进程会继承父进程的文件描述符。例：[MIT6.S081 lab的第三个求2-35内的素数](http://xv6.dgs.zone/labs/requirements/lab1.html)。
```c
#include <stidio.h>

void is_primes(int fd_father[2]){
    int fd_child[2];// 该进程的子进程的管道读写符
    pipe(fd_child);

    // 打印一定是素数的数（第一个写进来的数）
    int p = 0;
    read(fd_father[0], &p, sizeof(p));
    printf("prime %d\n", p);
    // 如果n是p的整数倍，那他一定不是素数，丢弃;否则，可能是素数，写给子进程。
    int n;
    int left = 0; // 剩下几个数字没有判断。
    while(read(fd_father[0], &n, sizeof(p)) != 0) {
        if(n % p != 0) {
            write(fd_child[1], &n, sizeof(n));
            left++;
        }
    }
    close(fd_father[0]);
    close(fd_child[1]);
    if(left == 0){ // 说明所有数都判断完了，为了避免最后一层还会执行后面的fork,避免出现多个0.
        exit(0);
    }
    //上面同理，先把自己进程的读端和子进程的写端用完，关掉，再开子进程。
    
    if(fork() == 0){ // 该进程的子进程
        is_primes(fd_child); // 子进程读取父进程进程写进来数字，并判断是否为素数：可能是素数的，写给孙进程然后递归调用，一定不是素数的，丢弃。
    }
    wait(0);
}

int main(int argc, char** atgv) {
    int fd_child[2]; // 子进程的管道读写端
    pipe(fd_child);

    // 先把数字写进子进程管道，并关掉写端。
    for(int i = 2; i <= 35; i++){
        write(fd_child[1], &i, sizeof(i));
    }
    close(fd_child[1]);
    
    // 创建子进程，只需要子进程的读端。
    if(fork() == 0) {  
        is_primes(fd_child); // 子进程读取父进程进程写进来数字，并判断是否为素数：可能是素数的，写给孙进程然后递归调用，一定不是素数的，丢弃。
    } 
    wait(0);
    exit(0);
}
```