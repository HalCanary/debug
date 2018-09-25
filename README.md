# Using GDB (or LLDB) #

https://sourceware.org/gdb/current/onlinedocs/gdb/

    c++ -g -o program program.cpp    

    gdb -q program

    gdb -q --args program foo bar

*Don't forget to compile with `-g`.*
    
## Important Commands: ##

  * quit (q)
  * help (h)
  * list (l) — list source code.
  * print (p) — print variable values
  * print /x (p/x) — print in hexadecimal
  * run (r) — start program from begining.  Try `run args < infile > outfile`. 
  * start — [gdb only] equivilent to `break main` followed by `run`.
  * break (b) — set breakpoint.  Try `break srcfile:line` or  `break functionname`.
  * step (s)  — continue program until control reaches a different source line.
  * next (n) — continue to the next source line in the innermost stack frame.
  * continue (c) — resume program execution. 
  * finish — finish running curent function.
  * watch — set watchpoint.  Break when the variable's value changes.
  * backtrace (bt) — print a stacktrace.
  * frame (f) — change your current frame.
  * up, down — move to next frame up or down.
  * [blank line] — repeat previous command.

## example 1 ##

    void set_to_zero(int* ptr) {
        *ptr = 0;
    }
    int main() {
        int* ptr = 0;
        set_to_zero(ptr);
    }

## example 2 ##

    #include <thread>
    #include <cstdio>
    void sleep(double seconds) {
        int64_t mics = 1000000 * seconds;
        auto dur = std::chrono::microseconds(mics);
        std::this_thread::sleep_for(dur);
    }
    int main() {
        int i = 0;
        while (1) {
            sleep(1);
            printf("%d\n", i++);
            fflush(stdout);
        }
    }

## example 3 ##

    #include <cassert>
    #include <cmath>
    #include <cstdio>
    void print_sqrt(int x) {
        assert(x >= 0);
        printf("%f\n", sqrt(x));
    }
    int main() {
        int i = -5;
        print_sqrt(i);
    }

## example 4 ##

    #include <cmath>
    #include <cstdio>

    static void error_msg(const char* file, int line, const char* msg) {
        fprintf(stderr, "%s:%d: %s\n", file, line, msg);
    }

    #define ABORT(msg) \
        do { error_msg(__FILE__, __LINE__, msg); *(int*)0xDEADBEEF = 1234; } while (0)
    #define ASSERT(cond) \
        do { if (!(cond)) { ABORT("assert failed: '" #cond "'."); } } while (0)

    void print_sqrt(int x) {
        ASSERT(x >= 0);
        printf("%f\n", sqrt(x));
    }
    int main() {
        int i = -5;
        print_sqrt(i);
        ABORT("abort");
    }
