
# Userfaultfd Tool

A tool for linux kernel exploit.

```c++
/*
 * Get a page that bind the exception handle.
 * If program read or write this page, then the exception handle will
 * interrupt operation with a thread lock until the function release_fault_page
 * be called. In the meantime, you have plenty of time to do what you want to do.
 * 
 * You can also set the hook to accomplish your exploit.
 **/
void *get_userfault_page(int num_pages);

/*
 * Default fcuntion, it is used to send semaphore.
 **/
void release_fault_page();

/*
 * When program happen page fault exception, it will call following hook in advance.
 * Default it is used to wait semaphore.
 **/
extern void (*require_fault_hook)(void);

/*
 * It is a hook in get_userfault_page, here it is used to initial semaphore default.
 **/
extern void (*init_fault_hook)(void);

/*
 * It can run your funtion with 5 parameters at most in a new thread.
 * Note: I only write x64 version.
 **/
void run_job(void (*job)(void *, ...), ...);

/*
 * The tool can print memory like hexdump,
 * mode 0 => only print BYTE
 * mode 1 => BYTE on left, and unsigned long long on right with hex format.
 **/
void print_hex(unsigned char *addr, int size, int mode);
```
