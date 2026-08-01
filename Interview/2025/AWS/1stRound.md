
# AWS Linux Cloud Support Engineer – Round 1 Interview Questions

1. Tell me about a time when you took on something significant outside your area of responsibility. Why was it important, and what was the outcome?

2. How did you complete that task even though you were not part of that team? Did you follow any SOPs?

3. What was the actual outcome of your contribution? Did you improve any processes or documentation?

4. Tell me about a time when you went above and beyond for a customer. Why did you do it, and what was the outcome?

5. Did your migration project complete before the deadline? What was the final result?

6. Server A cannot ping Server B. What could be the possible causes?

7. If the ping works, how would you check port connectivity?

8. If Telnet to port 22 fails and SSH is not working, what troubleshooting steps would you perform from the client side?

9. What does `traceroute` do?

10. What do the `*` (asterisk) symbols in `traceroute` output indicate?

11. Besides `telnet` and `traceroute`, what other client-side troubleshooting would you perform for SSH issues?

12. What does `ssh -vvv` help you troubleshoot?

13. What does an SSH **Connection timed out** error mean?

14. What are the common causes of an SSH **Connection timed out** error?

15. What does an SSH **Connection refused** error mean?

16. If you can SSH from Server C to Server B, but not from Server A to Server B, what troubleshooting would you perform on Server B?

17. Which log files would you check for SSH authentication issues?

18. What SSH configuration files or settings would you verify when one user can log in but another cannot?

19. As a normal user inside your own home directory, can you create files and directories?

20. What is the default permission of a newly created directory?

21. Can you `cd` into a directory with permission `600`? Why or why not?

22. How are default file and directory permissions determined in Linux?

23. What is `umask`?

24. What is the default permission of a newly created file?

25. What is an inode?

26. Do directories also have inode numbers?

27. Can a file and a directory have the same name in the same location? Why?

28. A Linux root filesystem is 99.5% full. How would you troubleshoot it?

29. If `/var/log` is consuming most of the disk space, what steps would you take?

30. If an application continuously generates log files and you are not permitted to delete them, how would you manage the situation?

31. If the root volume is completely full, how would you compress log files?

32. When would you decide to increase the disk size instead of cleaning files?

33. If `/boot` is consuming most of the disk space, how would you troubleshoot it?

34. How do you identify the currently running Linux kernel?

35. Why should you avoid deleting the currently running kernel?

36. If CPU utilization is very high, how would you troubleshoot the issue?

37. Which commands would you use to identify CPU-intensive processes?

38. What else can you analyze in `top` besides CPU usage?

39. What is a zombie process?

40. How do you identify zombie processes?

41. Can you kill a zombie process directly? If not, why?

42. What are Linux run levels?

43. Explain each Linux run level (0–6).

44. Is run level 4 really unused? Can it be customized?

45. How do you give full permissions to a file?

46. How do you check listening ports in Linux?

47. What is swap memory?

48. What is the purpose of swap?

49. Is swap size always twice the RAM size?

50. How do you mount a filesystem in Linux?

51. How do you unmount a filesystem?

52. Will a mounted filesystem persist after reboot? Why or why not?

53. How do you make a filesystem mount persistent?

54. How do you grant sudo privileges to a normal Linux user?

55. Which file controls sudo permissions?

56. Explain the complete Linux boot process.

57. What is the role of BIOS during boot?

58. What is the Master Boot Record (MBR)?

59. What are the components of the MBR?

60. What are the different Linux bootloaders?

61. What is the difference between GRUB, GRUB2, and LILO?

62. What is `initramfs` (or `initrd`)?

63. What is the first process started by the Linux kernel?

64. What is the difference between `init` and `systemd`?

65. Which parent process is used in RHEL 6 versus RHEL 7/8/9?

66. What is a kernel panic?

67. If the `initramfs` image is corrupted, how would you rebuild it?

68. Which command is used to regenerate the `initramfs` image?
