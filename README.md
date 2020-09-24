<div align="center">

## Dir


</div>

### Description

List subdirectories of the current directory.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[vsim](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/vsim.md)
**Level**          |Intermediate
**User Rating**    |4.0 (16 globes from 4 users)
**Compatibility**  |C, Microsoft Visual C\+\+, Borland C\+\+, UNIX C\+\+
**Category**       |[Complete Applications](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/complete-applications__3-7.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/vsim-dir__3-3047/archive/master.zip)





### Source Code

```
/* List subdirectories of the current directory */
/* DOS BORLAND C */
/*   Recommended compile: bcc -mt -tDc subs.c */
/* MS Visual C++ */
/* UNIX: SUN cc CONFIRMED: August 28, 1995 */
/*   Recommended compile: cc -o subs subs.c */
#include <stdio.h>
#include <time.h>
#if defined (__MSDOS__)
#include <stdlib.h>
#include <dir.h>
#include <dos.h>
#endif
#if defined (unix)
#include <dirent.h>
#include <sys/types.h>
#include <sys/stat.h>
#endif
#if defined (_MSC_VER)
#include <io.h>
#endif
static char poc[] = "vsim";
#if defined (__MSDOS__)         /* BORLAND C DOS */
int main(void)
{
  int i = 0, done;
  struct ffblk dta;
  time_t tnow;
  time(&tnow);
   printf("\n\t%s\n",ctime(&tnow));
   printf("\tDirectories:\n");
   printf("\t----------- \n");
   done = findfirst("*.*", &dta, FA_DIREC);
   while (!done){
     if (((dta.ff_attrib & FA_DIREC) == FA_DIREC) &&
        (dta.ff_name[0] != '.')){
        i++;
        printf("\t%s",dta.ff_name);
        if(dta.ff_name[8]=='\0')
         printf(" ");
        if(dta.ff_name[7]=='\0')
         printf(" ");
        if(dta.ff_name[6]=='\0')
         printf("  ");
        if(dta.ff_name[5]=='\0')
         printf("  ");
        if(dta.ff_name[4]=='\0')
         printf("   ");
        if(dta.ff_name[3]=='\0')
         printf("   ");
        if(dta.ff_name[2]=='\0')
         printf("    ");
        if(!(i%4))
         printf("\n");
      }
     done = findnext(&dta);
     }
  printf("\n");
  exit(0);
}
#endif
#if defined (unix)            /* UNIX */
main()
{
  DIR *dirp;
  struct dirent *dp;
  struct stat buf;
  time_t tnow;
  time(&tnow);
  printf("\n\t%s\n",ctime(&tnow));
  printf("\tDirectories:\n");
  printf("\t----------- \n");
  dirp = opendir(".");
  while (dp = readdir(dirp)) {
    if (stat(dp->d_name,&buf) == 0)
      if(buf.st_mode & S_IFDIR)
        printf("d %s\n", dp->d_name);
  }
  return closedir(dirp);
}
#endif
#if defined (_MSC_VER)          /* MS Visual C++ */
void main(void)
{
  struct _finddata_t c_file;
  long hFile;
  printf("Directories:\n");
  printf("----------- \n\n");
  /* Find first .c file in current directory */
  if((hFile = _findfirst("*.*", &c_file)) == -1L)
   printf("No subdirectories in current directory!\n");
  else{
   if((c_file.attrib & _A_SUBDIR )){
     printf("%-12s %.24s %9ld\n", c_file.name, ctime(&(c_file.time_write)), c_file.size);
     }
   /* Find the rest of the *.* files */
   while(_findnext(hFile, &c_file) == 0){
   if((c_file.attrib & _A_SUBDIR )){
     printf("%-12s %.24s %9ld\n", c_file.name, ctime(&(c_file.time_write)), c_file.size);
     }
   }
   _findclose(hFile);
  }
}
#endif
```

