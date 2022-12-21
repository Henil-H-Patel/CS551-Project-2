//   PROJECT_2 - GROUP - 2
//   GROUP MEMBERS : HENIL PATEL, VISESH JAIN, SHIVAM GUPTA
//   AIM : The aim of this document to provide the steps, which are required for the perform the system call and complie process
//   REFERENCE : For implementing the system call, and complie purpose, we have taken the reference 
//   1) Adding the new system call in kernel ( https://wiki.minix3.org/doku.php?id=developersguide:newkernelcall)
//   2) We have also taken the refernce of the class presentation, which was given by the one of the our classmates (https://blackboard.iit.edu/ultra/courses/_129784_1/cl/outline)
//   Using the following steps we can perform created and running the system call 
//   In this document we mention the flow of trap system call, and similarly we can perform the same for the rest of three system calls.


1> /usr/src/minix/include/minix/ipc.h

      //This is for storing the value of trap counter
      typedef struct {
      int num;
      uint8_t padding[52];
      } mess_pm_lsys_trapcounter;
      _ASSERT_MSG_SIZE(mess_pm_lsys_trapcounter);

      typedef struct noxfer_message {
      ...
      mess_pm_lsys_trapcounter m_pm_lsys_trapcounter;
      }

      //This is for storing the value of msg counter
      typedef struct {
      int num;
      uint8_t padding[52];
      } mess_pm_lsys_msgcounter;
      _ASSERT_MSG_SIZE(mess_pm_lsys_msgcounter);

      typedef struct noxfer_message {
      ...
      mess_pm_lsys_msgcounter m_pm_lsys_msgcounter;
      }
   
   
2> /* ADD THE KERNEL CALL (HERE WE REFER THE MINIX DOCUMENTATION AND CLASS PRESENTATION IN SYSTEM CALL ) */

      /usr/src/minix/include/minix/com.h

      #define SYS_TRAPCOUNTER ( KERNEL_CALL + 58 )
      #define SYS_INITTRAPCOUNTER ( KERNEL_CALL + 59 )
      #define SYS_MSGCOUNTER ( KERNEL_CALL + 60 )
      #define SYS_INITMSGCOUNTER ( KERNEL_CALL + 61 )

      TOTAL 62 // BASE NUMBER FOR KERNEL CALL

3> /usr/src/minix/kernel/system.c

      map(SYS_TRAPCOUNTER, do_trapcounter);
      map(SYS_INITTRAPCOUNTER, do_inittrapcounter);
      map(SYS_MSGCOUNTER, do_msgcounter);
      map(SYS_INITMSGCOUNTER, do_initmsgcounter);

4> /usr/src/minix/kernel/config.h

      #define USE_TRAPCOUNTER 1
      #define USE_INITTRAPCOUNTER 1
      #define USE_MSGCOUNTER 1
      #define USE_INITMSGCOUNTER 1

5> /usr/src/minix/kernel/system/do_trapcounter.c

   /* HERE WE IMPLEMENTED THE TRAPCOUNTER CODE IN C PROGRAMMING LANGUAGE */
   /* For your reference we attach the do_trapcounter.c and other relevant files in the zip folder so you can access it and test it */

6> /usr/src/minix/kernel/system.h

   /* The below is the trapcounter */

      int do_trapcounter(struct proc * caller, message *m_ptr);
      #if ! USE_TRAPCOUNTER
      #define do_trapcounter NULL
      #endif
   
   /* The below is the inittrapcounter */

      void do_inittrapcounter();
      #if ! USE_INITTRAPCOUNTER
      #define do_inittrapcounter NULL
      #endif

   /* The below is the for the message counter */

      int do_msgcounter(struct proc * caller, message *m_ptr);
      #if ! USE_MSGCOUNTER
      #define do_msgcounter NULL
      #endif

   /* The below is the for the initmsg counter */

      void do_initmsgcounter();
      #if ! USE_INITMSGCOUNTER
      #define do_initmsgcounter NULL
      #endif

/* NOW THE LIPSYS UNDERSTAND THE MESSAGE  */

7> usr/src/minix/include/minix/syslib.h

  /* The below is the file implementation in syslib.h, the below is the sys_trapcounter declaration */

      int sys_trapcounter(endpoint_t proc_cp, int *trapcounter);

  /* The below is the inittrapcounter declaration*/

      void sys_inittrapcounter();

  /* The below is the file implementation for msgcounter declaration */

      int sys_msgcounter( endpoint_t proc_cp, int *msgcounter);

   /* The below is the file implementation for initmsgcounter declaration */

      void sys_initmsgcounter();

8> usr/src/minix/kernel/system/Makefile.inc

   /* We include the do_trapcounter.c file in the Makefile.inc */

      do_statectl.c \
      do_trapcounter.c \

  /* as the same way we can include inittrapcounter in Makefile.inc */

      do_inittrapcounter.c \

      do_msgcounter.c \

  /* The initmsgcounter we put into the Makefile.inc */
  
      do_initmsgcounter.c

9> usr/src/minix/lib/libsys/Makefile

      sys_trace.c \
      
      sys_trapcounter.c \
  
  /* as the same way we can implement the another three system call */
  
      sys_inittrapcounter.c \
      
      sys_msgcounter.c \
      
      sys_initmsgcounter.c

10> usr/src/minix/lib/libsys/

  /* HERE WE NEED TO CREATE THE CODE FOR SYS */
  /* THEN THE FILE PATH MUST BE usr/src/minix/lib/libsys/sys_trapcounter.c */
  /* WE FOLLOWED THE SAME PROCEDURE FOR THE usr/src/minix/lib/libsys/sys_inittrapcounter.c */
  /* We provide you file in zip folder for your reference */
  /* NOW THE PRIVILEDGE SYSTEM SERVER CHANGE */
  
11> usr/src/minix/include/minix/callnr.h

      #define PM_TRAPCOUNTER ( PM_BASE + 48 )
      
      #define PM_INTTRAPCOUNTER ( PM_BASE + 49 )
      
      #define PM_MSGCOUNTER ( PM_BASE + 50 )
      
      #define PM_INITMSGCOUNTER ( PM_BASE + 51 )
      
      TOTAL 52
      /* HIGHEST NUMBER */

12> usr/src/minix/servers/pm/Makefile
 
      profile trapcounter.c inittrapcounter.c msgcounter.c initmsgcounter.c

13> usr/src/minix/servers/pm/trapcounter.c

 /* in this trapcounter.c, we put the code for the execution */
 /* we can implement the inittrapcounter.c file as the same way as we above used it */
 /* like usr/src/minix/servers/pm/inittrapcounter.c */

14> usr/src/minix/servers/pm/

 /* in this file we define the proto type of the function, we want to perform during the system call */
 /* here we define the two proto type */
 /* this changes happened in proto.h file */

      int do_trapcounter(void);
      
      void do_inittrapcounter(void);
      
      int do_msgcounter(void);
      
      void do_initmsgcounter(void);

15> usr/src/minix/server/pm

 /* here we need add the call into the table.c file */
 /* the same thing is happened with the inittrapcounter */
      
      CALL(PM_TRAPCOUNTER) = do_trapcounter
               
      CALL(PM_INITTRAPCOUNTER) = do_inittrapcounter
               
      CALL(PM_MSGCOUNTER) = do_msgcounter
               
      CALL(PM_INITMSGCOUNTER) = do_initmsgcounter        

      16> /usr/src/distrib/sets/lists/minix-comp/mi

16> usr/include/sys/trapcounter.c (minix-com)
 /* we can perform the inittrapcounter operation using the same command */
      
      /usr/include/sys/inittrapcounter.c (minix-com)
 
 /* we have followed the same technique for the other two system call */
 
17> /usr/src/minix/lib/libc/sys/

 /* In this directory we change the Makefile.inc */
 /* In this Makefile.inc we do the below changes */
 /* setgpid.c we put the trapcounter.c and inittrapcounter.c after the setgpid */

18> /usr/src/minix/lib/libc/sys

 /* In this sys directory we add the inittrapcounter.c */
 /* We do the same thing for the other three */

19> /usr/src/sys/sys/

 /* In this sys directory we define the inittrapcounter.c */
 /* defined inittrapcounter.c */

20> /usr/src/sys/sys/

 /* In this sys directory we create the inittrapcounter.h */
 /* The commmand looks like usr/src/sys/sys/inittrapcounter.h */
 /* The same way we can add the other function */
 
      #ifdenf _SYS_trapcounter_h
      #define _SYS_trapcounter_h
      int trapcounter(void);
      #endif

      #ifdenf _SYS_inittrapcounter_h
      #define _SYS_inittrapcounter_h
      void  inittrapcounter(void);
      #endif
      
      #ifdenf _SYS_msgcounter_h
      #define _SYS_msgcounter_h
      int msgcounter(void);
      #endif
      
      #ifdenf _SYS_initmsgcounter_h
      #define _SYS_initmsgcounter_h
      void initmsgcounter(void);
      #endif

21> /usr/src/minix/tests

 /* In the tests we creatd the inittrapcounter.c */
 /* /usr/src/minix/tests/inittrapcounter.c */

22> /usr/src/minix/tests

 /* In the tests directory /Makefile we add the inittrapcounter_tests */
 /* usr/src/minix/tests/inittrapcounter_tests */

23> /usr/src/minix/commands/minix-service

      { "TRAPCOUNTER", SYS_TRAPCOUNTER};
      { "INITTRAPCOUNTER", SYS_INITTRAPCOUNTER};
      { "MSGCOUNTER", SYS_MSGCOUNTER};
      { "INITMSGCOUNTER", SYS_INITMSGCOUNTER}   

 /* Now do make;make install */

24> /usr/src/include

 /* In this directory we gone through the unistd.h */
 
      int trapcounter(void);
      void inittrapcounter(void);
      int msgcounter(void);
      void initmsgcounter(void);


