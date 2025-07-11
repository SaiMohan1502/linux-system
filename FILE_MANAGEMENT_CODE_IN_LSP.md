## Write a C program to create a new text file and write user input to it.

```c
#include <stdio.h>

int main() {
    FILE *f = fopen("newfile.txt", "w");
    char input[100];
    printf("Enter text to write: ");
    fgets(input, sizeof(input), stdin);
    fputs(input, f);
    fclose(f);
    return 0;
}
```
#Develop a C program to open an existing text file and display its contents

```c
#include <stdio.h>

int main() {
    FILE *f = fopen("output.txt", "r"); //for better experience run this program in linux
    char ch;
    if (f == NULL) {
        printf("File not found!\n");
        return 1;
    }
    while ((ch = fgetc(f)) != EOF) {
        putchar(ch);
    }
    fclose(f);
    return 0;
}
```

#Implement a C program to create a new directory named "Test" in the current directory?

```c
 #include <stdio.h>
#include <sys/stat.h>

int main() {
    if (mkdir("Test1523", 1024) == 0)
        printf("Directory created.\n");
    else
        printf("Failed to create directory.\n");
    return 0;
} 

                // OR //

#include <stdio.h>
#include <sys/stat.h>
int main() {
    // Step 1: Create the directory
    if (mkdir("Test", 0777) == 0)
        printf("Directory 'Test' created.\n");
    else
        printf("Directory already exists or creation failed.\n");

    // Step 2: Create and write to the file inside "Test"
    FILE *f = fopen("note.txt", "w");
    if (f == NULL) {
        printf("Failed to create file.\n");
        return 1;
    }
    fputs("This is a sample text inside note.txt.\n", f);
    fclose(f);
    printf("File 'note.txt' created and written inside 'Test' directory.\n");

    // Step 3: Open and read the file
    f = fopen("note.txt", "r");
    if (f == NULL) {
        printf("Failed to open file for reading.\n");
        return 1;
    }
    printf("Reading contents of note.txt:\n");
    char ch;
    while ((ch = fgetc(f)) != EOF) {
        putchar(ch);
    }
    fclose(f);
    return 0;
}
```

#Write a C program to check if a file named "sample.txt" exists in the current directory?

```c
#include <stdio.h>
#include <stdlib.h>

int main() 
{
    FILE *fi = fopen("sample.txt", "r");
    if (fi) 
    {
        printf("File exists.\n");
        fclose(fi);
    } else 
    {
        printf("File does not exist.\n");
    }
    return 0;
}
```
#Develop a C program to rename a file from "oldname.txt" to "newname.txt"?

```c
#include <stdio.h>

int main() 
{
    if (rename("your_old_file_name.txt", "new_name_to_your_file.txt") == 0)
        printf("File renamed.\n");
    else
        printf("Rename failed.\n");
    return 0;
}
```

# Implement a C program to delete a file named "delete_me.txt"?

```c
#include <stdio.h>

int main() 
{
    if (remove("delete_me.txt") == 0)
        printf("File deleted.\n");
    else
        printf("Delete failed.\n");
    return 0;
}
```

# Write a C program to copy the contents of one file to another?

```c
#include <stdio.h>

int main() {
    FILE *source, *destination;
    char ch;

    source = fopen("source.txt", "r");
    destination = fopen("copy.txt", "w");

    if (source == NULL || destination == NULL) {
        printf("Error: Unable to open file.\n");
        return 1;
    }

    while ((ch = fgetc(source)) != EOF) {
        fputc(ch, destination);
    }

    printf("File copied successfully.\n");

    fclose(source);
    fclose(destination);

    return 0;
}
```

#Develop a C program to move a file from one directory to another?

```c
#include <stdio.h>
#include <sys/stat.h>

int main() 
{
    // Step 1: Create two directories
    mkdir("Dir1", 0777);
    mkdir("Dir2", 0777);

    // Step 2: Create a file in Dir1
    FILE *f = fopen("Dir1_test.txt", "w");
    if (f == NULL) {
        printf("Failed to create file in Dir1.\n");
        return 1;
    }
    fputs("This file will be moved to Dir2.\n", f);
    fclose(f);
    printf("File 'test.txt' created in 'Dir1'.\n");

    // Step 3: Move file from Dir1 to Dir2
    if (rename("Dir1_test.txt", "Dir2_test.txt") == 0)
        printf("File moved from 'Dir1' to 'Dir2' successfully.\n");
    else
        printf("Failed to move file.\n");

    return 0;
}
```
#Implement a C program to list all files in the current directory?

```c
#include <stdio.h>
#include <dirent.h>

int main() {
    struct dirent *d;
    DIR *dir = opendir(".");
    if (dir) 
    {
        while ((d = readdir(dir)) != NULL) {
            printf("%s\n", d->d_name);
        }
        closedir(dir);
    } else {
        printf("Unable to open directory.\n");
    }
    return 0;
}
```

# Write a C program to get the size of a file named "file.txt"?

```c
#include <stdio.h>

int main() {
    FILE *f = fopen("YOUR TEXT FILE NAME ", "r");
    if (!f) {
        printf("File not found!\n");
        return 1;
    }
    fseek(f, 0, SEEK_END);
    long size = ftell(f);
    fclose(f);
    printf("Size: %ld bytes\n", size);
    return 0;
}
```

#Develop a C program to check if a directory named "Test" exists in the current directory?

```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    struct stat st;

    if (stat("Test", &st) == 0 && S_ISDIR(st.st_mode)) {
        printf("Directory 'Test' exists.\n");
    } else {
        printf("Directory 'Test' does not exist.\n");
    }

    return 0;
}
```

#Implement a C program to create a new directory named "Backup" in the parent directory?

```c
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>

int main() {
    int result;

    // "../Backup" refers to the parent directory
    result = mkdir("../Backup", 0777);  // Full permissions

    if (result == 0)
        printf("Directory 'Backup' created in the parent directory.\n");
    else
        perror("Failed to create directory");

    return 0;
}
```

# Write a C program to recursively list all files and directories in a given directory?

```c
#include <stdio.h>
#include <dirent.h>
#include <sys/stat.h>
#include <string.h>

void list(const char *path) {
    DIR *d = opendir(path);
    struct dirent *e;
    char p[512];

    if (!d) return;

    while ((e = readdir(d))) {
        if (strcmp(e->d_name, ".") == 0 || strcmp(e->d_name, "..") == 0)
            continue;

        snprintf(p, sizeof(p), "%s/%s", path, e->d_name);
        printf("%s\n", p);

        struct stat st;
        if (!stat(p, &st) && S_ISDIR(st.st_mode))
            list(p);
    }
    closedir(d);
}

int main() {
    list(".");
    return 0;
}
```

# Develop a C program to delete all files in a directory named "Temp"?

```c
#include <stdio.h>
#include <dirent.h>
#include <unistd.h>
#include <string.h>

int main() {
    DIR *dir = opendir("Temp");
    struct dirent *entry;
    char path[256];

    if (!dir) {
        perror("Could not open Temp");
        return 1;
    }

    while ((entry = readdir(dir))) {
        if (entry->d_type == DT_REG) {  // Only regular files
            snprintf(path, sizeof(path), "Temp/%s", entry->d_name);
            remove(path);
            printf("Deleted: %s\n", path);
        }
    }

    closedir(dir);
    return 0;
}
```

#Implement a C program to count the number of lines in a file named "data.txt"? 

```c
#include <stdio.h>

int main() {
    FILE *file = fopen("data.txt", "r");
    int lines = 0;
    char ch;

    if (file == NULL) {
        printf("Could not open data.txt\n");
        return 1;
    }

    while ((ch = fgetc(file)) != EOF) {
        if (ch == '\n')
            lines++;
    }

    fclose(file);
    printf("Total lines: %d\n", lines);
    return 0;
}
```

#Write a C program to append "Goodbye!" to the end of an existing file named "message.txt"?

```c
#include <stdio.h>

int main() {
    FILE *file = fopen("message.txt", "a");  // Open in append mode

    if (file == NULL) {
        printf("Could not open message.txt\n");
        return 1;
    }

    fputs("Goodbye!\n", file);  // Append the text
    fclose(file);

    printf("Appended 'Goodbye!' to message.txt\n");
    return 0;
}
```

#Implement a C program to change the permissions of a file named "file.txt" to readonly?

```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    int result = chmod("file.txt", 0444);  // Read-only permission

    if (result == 0)
        printf("Permissions changed to read-only for file.txt\n");
    else
        perror("chmod failed");

    return 0;
}
```

# Write a C program to change the ownership of a file named "file.txt" to the user "user1"?

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <unistd.h>
#include <pwd.h>

int main() {
    const char *filename = "file.txt";
    const char *username = "user1";

    struct passwd *pw = getpwnam(username);
    if (pw == NULL) {
        printf("User '%s' not found.\n", username);
        return 1;
    }

    if (chown(filename, pw->pw_uid, pw->pw_gid) == 0)
        printf("Ownership of %s changed to user '%s'.\n", filename, username);
    else
        perror("chown failed");

    return 0;
}

	Note: This program must be run as root (sudo) to change ownership.
		gcc chown_file.c -o chown_file
			sudo ./chown_file
```
# Develop a C program to get the last modified timestamp of a file named "file.txt"?

```c
#include <stdio.h>
#include <sys/stat.h>
#include <time.h>

int main() {
    struct stat fileStat;

    if (stat("file.txt", &fileStat) == -1) {
        perror("stat failed");
        return 1;
    }

    printf("Last modified time: %s", ctime(&fileStat.st_mtime));
    return 0;
}
```
# Implement a C program to create a temporary file and write some data to it?

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *tmp = tmpfile();  // Create a temporary file

    if (tmp == NULL) {
        perror("tmpfile failed");
        return 1;
    }

    fputs("Temporary data written to file.\n", tmp);
    rewind(tmp);  // Go back to the beginning

    char buffer[100];
    fgets(buffer, sizeof(buffer), tmp);  // Read back the data

    printf("Read from temp file: %s", buffer);

    // No need to manually delete — temp file auto-deleted when closed
    fclose(tmp);
    return 0;
}
```

# Write a C program to check if a given path refers to a file or a directory?

```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    char path[100];
    struct stat s;

    // Input path from user
    printf("Enter the path: ");
    scanf("%s", path);

    // Use stat() to get info
    if (stat(path, &s) == 0) {
        if (S_ISREG(s.st_mode))
            printf("It is a file.\n");
        else if (S_ISDIR(s.st_mode))
            printf("It is a directory.\n");
        else
            printf("It is neither a regular file nor a directory.\n");
    } else {
        printf("Path does not exist.\n");
    }

    return 0;
}
```

# Develop a C program to create a hard link named "hardlink.txt" to a file named "source.txt"?

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    if (link("source.txt", "hardlink.txt") == 0)
        printf("Hard link created successfully.\n");
    else
        perror("Failed to create hard link");

    return 0;
}
```
# Implement a C program to read and display the contents of a CSV file named "data.csv"?

```c
#include <stdio.h>

int main() {
    FILE *f = fopen("data.csv", "r");
    char line[1024];

    if (f == NULL) {
        printf("Failed to open data.csv\n");
        return 1;
    }

    printf("Contents of data.csv:\n\n");

    while (fgets(line, sizeof(line), f)) {
        printf("%s", line);  // Print each line as it is
    }

    fclose(f);
    return 0;
}
```
# Write a C program to get the absolute path of the current working directory? 

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    char path[1024];

    // Get current working directory
    if (getcwd(path, sizeof(path)) != NULL)
        printf("Current working directory: %s\n", path);
    else
        perror("getcwd() error");

    return 0;
}
```

# Develop a C program to get the size of a directory named "Documents"?

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>
#include <sys/stat.h>

int main() {
    DIR *dir;
    struct dirent *entry;
    struct stat info;
    char filepath[512];
    long totalSize = 0;
    int fileCount = 0;

    dir = opendir("Documents");
    if (dir == NULL) {
        printf("Could not open 'Documents' directory.\n");
        return 1;
    }

    while ((entry = readdir(dir)) != NULL) {
        // Skip "." and ".."
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;

        // Make full path: Documents/filename
        snprintf(filepath, sizeof(filepath), "Documents/%s", entry->d_name);

        // Get file info
        if (stat(filepath, &info) == 0 && S_ISREG(info.st_mode)) {
            totalSize += info.st_size;
            fileCount++;
        }
    }

    closedir(dir);

    // Convert sizes
    float sizeKB = totalSize / 1024.0;
    float sizeMB = sizeKB / 1024.0;

    // Print results
    printf("Total files      : %d\n", fileCount);
    printf("Total size (B)   : %ld bytes\n", totalSize);
    printf("Total size (KB)  : %.2f KB\n", sizeKB);
    printf("Total size (MB)  : %.2f MB\n", sizeMB);
    return 0;
}
```
# Implement a C program to recursively copy all files and directories from one directory to another?

```c
#include <stdio.h>
#include <dirent.h>
#include <sys/stat.h>
#include <string.h>

void copyFile(const char *src, const char *dst) {
    FILE *f1 = fopen(src, "rb"), *f2 = fopen(dst, "wb");
    char buf[1024];
    size_t n;
    if (!f1 || !f2) return;
    while ((n = fread(buf, 1, sizeof(buf), f1)) > 0)
        fwrite(buf, 1, n, f2);
    fclose(f1); fclose(f2);
}

void copyDir(const char *src, const char *dst) {
    mkdir(dst, 0777);
    DIR *d = opendir(src);
    struct dirent *e;
    char p1[512], p2[512];
    while ((e = readdir(d))) {
        if (strcmp(e->d_name, ".") == 0 || strcmp(e->d_name, "..") == 0) continue;
        snprintf(p1, sizeof(p1), "%s/%s", src, e->d_name);
        snprintf(p2, sizeof(p2), "%s/%s", dst, e->d_name);
        struct stat s;
        stat(p1, &s);
        if (S_ISDIR(s.st_mode))
            copyDir(p1, p2);
        else if (S_ISREG(s.st_mode))
            copyFile(p1, p2);
    }
    closedir(d);
}

int main() {
    char src[100], dst[100];
    printf("Source folder: ");
    scanf("%s", src);
    printf("Destination folder: ");
    scanf("%s", dst);
    copyDir(src, dst);
    printf("Copy complete.\n");
    return 0;
}
```
# Write a C program to get the number of files in a directory named "Images"?

```c
#include <stdio.h>
#include <dirent.h>
#include <sys/stat.h>
#include <string.h>

int main() {
    DIR *dir;
    struct dirent *entry;
    struct stat info;
    char path[256];
    int count = 0;

    dir = opendir("Images");
    if (dir == NULL) {
        printf("Could not open 'Images' directory.\n");
        return 1;
    }

    while ((entry = readdir(dir)) != NULL) {
        // Skip "." and ".."
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;

        // Build full file path
        snprintf(path, sizeof(path), "Images/%s", entry->d_name);

        // Check if it's a regular file
        if (stat(path, &info) == 0 && S_ISREG(info.st_mode)) {
            count++;
        }
    }

    closedir(dir);

    printf("Number of files in 'Images': %d\n", count);
    return 0;
}
```
# Develop a C program to create a FIFO (named pipe) named "myfifo" in the current directory?

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>

int main() {
    const char *fifo_path = "myfifo";

    // Create the named pipe (FIFO) with read/write permission for all
    if (mkfifo(fifo_path, 0666) == 0)
        printf("FIFO 'myfifo' created successfully.\n");
    else
        perror("Failed to create FIFO");

    return 0;
}
```
# Implement a C program to read data from a FIFO named "myfifo"? 

```c
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>

int main() {
    int fd;
    char buffer[100];

    // Open the FIFO for Reading
    fd = open("myfifo", O_RDONLY); //O_WRONLY FOR WRITE // O_RDWR FOR READ-&-WRITE
    if (fd < 0) {
        perror("Failed to open FIFO");
        return 1;
    }

    // Read from the FIFO
    read(fd, buffer, sizeof(buffer));  // FOR WRITE USE  write(fd, msg, sizeof(msg)); //USE WRITE & READ BOTH FOR READ_WRITE
    
    printf("Data read from FIFO: %s\n", buffer);

    close(fd);
    return 0;
}


COMMANDS FOR CREATING THE FIFO FILE 

STEP_1 -> mkfifo myfifo // CREATE FIFO_FILE

STEP_2 -> echo "Hello from FIFO!" > myfifo // TEXT IN FIFO_FILE

```

# Write a C program to truncate a file named "file.txt" to a specified length?

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    const char *filename = "file.txt";
    int length;

    printf("Enter the length to truncate the file to: ");
    scanf("%d", &length);

    // Truncate the file
    if (truncate(filename, length) == 0) {
        printf("File '%s' truncated to %d bytes successfully.\n", filename, length);
    } else {
        perror("Truncation failed");
    }

    return 0;
}

MAKE SURE FILE EXIST, IF NOT DO AS FOLLOWS 

STEP_1 -> echo "This is a sample file for truncation" > file.txt
    where,    
        Creates file.txt and writes " This is a sample file for truncation "
```

# Develop a C program to search for a specific string in a file named "data.txt" and display the line number(s) where it occurs? 

```c
#include <stdio.h>
#include <string.h>

int main() {
    FILE *f = fopen("data.txt", "r");
    char line[256], word[50];
    int line_num = 0;

    if (!f) return printf("File not found.\n"), 1;

    printf("Enter word to search: ");
    scanf("%s", word);

    while (fgets(line, sizeof(line), f)) {
        line_num++;
        if (strstr(line, word))
            printf("Found on line %d\n", line_num);
    }

    fclose(f);
    return 0;
}
```
# Implement a C program to get the file type (regular file, directory, symbolic link, etc.) of a given path?

```c
#include <stdio.h>
#include <sys/stat.h>
#include <unistd.h>

int main(void) 
{
  struct stat fileStat;
  char path[256];
  // Get input path from user
  printf("Enter the path: ");
  scanf("%s", path);
  // Use lstat to get file info (handles symbolic links too)
  if(lstat(path, &fileStat) == -1) 
  {
    perror("lstat failed");
    return 1;
  }
  // Determine the file type
  if (S_ISREG(fileStat.st_mode))
     printf("It is a regular file.\n");
  else if (S_ISDIR(fileStat.st_mode))
     printf("It is a directory.\n");
  else if (S_ISLNK(fileStat.st_mode))
     printf("It is a symbolic link.\n");
  else if (S_ISCHR(fileStat.st_mode))
     printf("It is a character device file.\n");
  else if (S_ISBLK(fileStat.st_mode))
     printf("It is a block device file.\n");
  else if (S_ISFIFO(fileStat.st_mode))
     printf("It is a FIFO/pipe.\n");
  else if (S_ISSOCK(fileStat.st_mode))
     printf("It is a socket.\n");
  else
     printf("Unknown file type.\n");
     return 0;
}
```

# Write a C program to create a new empty file named "empty.txt"? 

```c
#include <stdio.h>

int main() {
    FILE *file = fopen("empty.txt", "w");  // "w" creates/overwrites file
    if (file == NULL) {
        printf("Failed to create file.\n");
        return 1;
    }

    printf("File 'empty.txt' created successfully.\n");
    fclose(file);
    return 0;
}
```

# Develop a C program to get the permissions (mode) of a file named "file.txt"? 

```c
#include <stdio.h>
#include <sys/stat.h>

void print_permissions(mode_t mode) {
    printf("Permissions: ");

    // User permissions
    printf( (mode & S_IRUSR) ? "r" : "-");
    printf( (mode & S_IWUSR) ? "w" : "-");
    printf( (mode & S_IXUSR) ? "x" : "-");

    // Group permissions
    printf( (mode & S_IRGRP) ? "r" : "-");
    printf( (mode & S_IWGRP) ? "w" : "-");
    printf( (mode & S_IXGRP) ? "x" : "-");

    // Others permissions
    printf( (mode & S_IROTH) ? "r" : "-");
    printf( (mode & S_IWOTH) ? "w" : "-");
    printf( (mode & S_IXOTH) ? "x" : "-");

    printf("\n");
}

int main() {
    struct stat fileStat;

    if (stat("file.txt", &fileStat) < 0) {
        perror("Could not get file info");
        return 1;
    }

    print_permissions(fileStat.st_mode);
    return 0;
}



    WHERE,
        S_I -> " STATIC_INDICATOR"
        USR -> " USER "
        GRP -> " GROUP "
        OTH -> " OTHERS "
        R, W, X -> READ, WRITE, EXECUTE 
    
```

# Implement a C program to recursively delete a directory named "Temp" and all its contents?

```c
#include <stdio.h>
#include <dirent.h>
#include <unistd.h>
#include <string.h>
#include <sys/stat.h>

void delDir(const char *path) {
    DIR *d = opendir(path);
    struct dirent *e;
    char full[512];
    struct stat st;

    if (!d) return;

    while ((e = readdir(d))) {
        if (strcmp(e->d_name, ".") == 0 || strcmp(e->d_name, "..") == 0) continue;

        snprintf(full, sizeof(full), "%s/%s", path, e->d_name);
        stat(full, &st);

        if (S_ISDIR(st.st_mode))
            delDir(full);    // Recursive delete for subfolder
        else
            remove(full);    // Delete file
    }

    closedir(d);
    rmdir(path);             // Remove now-empty folder
    printf("Deleted: %s\n", path);
}

int main() {
    delDir("Temp");
    return 0;
}
 ```

# Write a C program to read and display the first 10 lines of a file named "log.txt"? 

```c
#include <stdio.h>

int main() {
    FILE *file = fopen("log.txt", "r");
    char line[256];
    int count = 0;

    if (!file) {
        printf("Could not open file.\n");
        return 1;
    }

    while (fgets(line, sizeof(line), file) && count < 10) {
        printf("%s", line);
        count++;
    }

    fclose(file);
    return 0;
}


seq 1 20 > log.txt // SIMPLE CMD FOR CREATING SAMPLE LOG TEXT FILE

```

# Develop a C program to read data from a text file named "input.txt" and write it to another file named "output.txt" in reverse order?

```c
#include <stdio.h>
#include <string.h>

#define MAX 1000
#define LEN 256

int main() {
    FILE *in = fopen("input.txt", "r");
    FILE *out = fopen("output.txt", "w");
    char lines[MAX][LEN];
    int count = 0;

    if (!in || !out) {
        printf("Error opening files.\n");
        return 1;
    }

    // Read each line from input.txt
    while (fgets(lines[count], LEN, in) && count < MAX)
        count++;

    // Write lines in reverse to output.txt
    for (int i = count - 1; i >= 0; i--)
        fputs(lines[i], out);

    printf("Reversed content written to 'output.txt'.\n");

    fclose(in);
    fclose(out);
    return 0;
}
```

# Implement a C program to create a new directory named with the current date in the format "DD-MM-YYYY-HH-MM-SS"?

```c
#include <stdio.h>
#include <time.h>
#include <sys/stat.h>

int main() {
    char dirname[100];
    time_t now = time(NULL);
    struct tm *t = localtime(&now);

    strftime(dirname, sizeof(dirname), "%d-%m-%Y-%H-%M-%S", t);
    if (mkdir(dirname, 0755) == 0)
        printf("Directory '%s' created.\n", dirname);
    else
        perror("Failed");

    return 0;
}
```

# Write a C program to read and display the contents of a binary file named "binary.bin"?

```c
#include <stdio.h>

int main() {
    FILE *file = fopen("binary.bin", "rb");
    unsigned char byte;

    if (!file) {
        printf("Could not open 'binary.bin'\n");
        return 1;
    }

    printf("Contents of binary.bin (in hex):\n");

    while (fread(&byte, sizeof(byte), 1, file) == 1) {
        printf("%02X ", byte);  // print in hex format
    }

    printf("\n");
    fclose(file);
    return 0;
}

  FOR CREATING A BINARY-BIN FILE IN C CODE

        int num = 254555;
        fwrite(&num, sizeof(num), 1, file);   
```

# Develop a C program to get the size of the largest file in a directory?

```c
#include <stdio.h>
#include <dirent.h>
#include <sys/stat.h>
#include <string.h>

int main() {
    DIR *d = opendir(".");
    struct dirent *e;
    struct stat s;
    char bigfile[256] = "";
    long max = 0;

    while ((e = readdir(d))) {
        stat(e->d_name, &s);
        if (S_ISREG(s.st_mode) && s.st_size > max) {
            max = s.st_size;
            strcpy(bigfile, e->d_name);
        }
    }

    closedir(d);
    if (strlen(bigfile))
        printf("Largest: %s\nSize: %ld bytes | %.2f KB | %.2f MB\n",
               bigfile, max, max / 1024.0, max / (1024.0 * 1024.0));
    else
        printf("No files found.\n");

    return 0;
}
```

# Implement a C program to check if a file named "data.txt" is readable? 

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    if (access("data.txt", R_OK) == 0)
        printf("File 'data.txt' is readable.\n");
    else
        printf("File 'data.txt' is NOT readable.\n");
}


    access("data.txt", R_OK) checks if the file is readable.
    Returns 0  → yes, readable.
    Returns -1 → not readable or doesn't exist.
```

# Write a C program to create a new directory named "Logs" and move all files with the ".log" extension into it?

```c
#include <stdio.h>
#include <dirent.h>
#include <string.h>
#include <sys/stat.h>

int main() {
    DIR *dir = opendir(".");
    struct dirent *entry;
    char oldname[256], newname[300];

    // Create Logs directory if it doesn't exist
    mkdir("Logs", 0755);

    if (!dir) {
        perror("Cannot open current directory");
        return 1;
    }

    while ((entry = readdir(dir)) != NULL) {
        // Check if it's a regular file and ends with ".log"
        if (strstr(entry->d_name, ".log")) {
            snprintf(oldname, sizeof(oldname), "%s", entry->d_name);
            snprintf(newname, sizeof(newname), "Logs/%s", entry->d_name);

            if (rename(oldname, newname) == 0)
                printf("Moved: %s -> %s\n", oldname, newname);
            else
                perror("Error moving file");
        }
    }

    closedir(dir);
    return 0;
}
```

# Develop a C program to check if a file named "config.ini" is writable?

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    if (access("config.ini", W_OK) == 0)
        printf("File 'config.ini' is writable.\n");
    else
        printf("File 'config.ini' is NOT writable.\n");

    return 0;
}

 
    WHERE,
        R_OK -> IS TO CHECK FOR READ PERMISSION
        W_OK -> IS TO CHECK FOR WRITE PERMISSION
        R_OK | W_OK -> IS TO CHECK FOR BOTH READ & WRITE PERMISSION
    ALSO,
        Returns 0 → Writable / Reable / Both (Read & Write) 
        Returns -1 → Not writable or doesn’t exist

```

# Write a C program to get the number of hard links to a file named "file.txt"?

```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    struct stat info;

    if (stat("file.txt", &info) == 0) {
        printf("Number of hard links to 'file.txt': %lu\n", info.st_nlink);
    } else {
        perror("Error getting file info");
    }

    return 0;
}

 
	COMMAND TO CREATE A HARDLINK TO THE TXT FILE AS  FOLLOWS  
    	     ->  ln file.txt hardlink.txt

```

# Develop a C program to copy the contents of all text files in a directory into a single file named "combined.txt"?

```c
#include <stdio.h>
#include <dirent.h>
#include <string.h>

int main() {
    FILE *out = fopen("combined.txt", "w");
    DIR *d = opendir(".");
    struct dirent *e;
    char line[256];
    FILE *in;

    while ((e = readdir(d))) {
        if (strstr(e->d_name, ".txt") && strcmp(e->d_name, "combined.txt") != 0) {
            in = fopen(e->d_name, "r");
            if (in) {
                while (fgets(line, sizeof(line), in))
                    fputs(line, out);
                fclose(in);
            }
        }
    }
    closedir(d);
    fclose(out);
    printf("All text files merged into 'combined.txt'.\n");
    return 0;
}
```

# Implement a C program to recursively calculate the total size of all files in a directory and its subdirectories?

```c
#include <stdio.h>
#include <dirent.h>
#include <sys/stat.h>
#include <string.h>

long get_size(const char *path) {
    DIR *d = opendir(path);
    struct dirent *e;
    struct stat st;
    char full[512];
    long total = 0;

    if (!d) return 0;

    while ((e = readdir(d))) {
        if (strcmp(e->d_name, ".") == 0 || strcmp(e->d_name, "..") == 0) continue;
        snprintf(full, sizeof(full), "%s/%s", path, e->d_name);
        stat(full, &st);

        if (S_ISDIR(st.st_mode))
            total += get_size(full);
        else
            total += st.st_size;
    }

    closedir(d);
    return total;
}

int main() {
    long size = get_size(".");
    printf("Total size: %ld bytes\n", size);
    return 0;
}
 ```

# Write a C program to get the number of bytes in a file named "data.bin"?

```c
#include <stdio.h>

int main() {
    FILE *f = fopen("data.bin", "rb");
    if (!f) return printf("Cannot open file.\n"), 1;

    fseek(f, 0, SEEK_END);
    long size = ftell(f);
    fclose(f);

    printf("Size of 'data.bin': %ld bytes\n", size);
    return 0;
}
```

# Implement a C program to open a file named "data.txt" in read mode and display its contents?

```c
#include <stdio.h>

int main() {
    FILE *file;
    char ch;

    file = fopen("data.txt", "r");  // Open file in read mode

    if (file == NULL) {
        printf("Error: Could not open data.txt\n");
        return 1;
    }

    printf("Contents of data.txt:\n");
    while ((ch = fgetc(file)) != EOF) {
        putchar(ch);  // Display character to console
    }

    fclose(file);  // Close the file
    return 0;
}
```

# Write a C program to create a symbolic link named "link.txt" to a file named "target.txt"?

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    if (symlink("target.txt", "link.txt") == 0)
        printf("Symbolic link created.\n");
    else
        perror("Failed");

    return 0;
}

 
 WE CAN CHECK THE LINK BY USING THE BELOW COMMANDS
     1 -> ln -s target.txt link.txt // creates direct LINK
     2 -> ls -l link.txt
        where, 
           The l at the beginning (lrwxrwxrwx) means it’s a symbolic link
           The symbol -> target.txt shows where the link points to
```


# Develop a C program to read data from a binary file named "data.bin" and display it in hexadecimal format?
  
```c
#include <stdio.h>

void print_binary(unsigned char b) {
    for (int i = 7; i >= 0; i--)
        printf("%d", (b >> i) & 1);
}

int main() {
    FILE *f = fopen("data.bin", "rb");
    unsigned char b;

    if (!f) return printf("Cannot open file.\n"), 1;

    printf("DEC\tOCT\tHEX\tBIN\n");
    while (fread(&b, 1, 1, f) == 1) {
        printf("%3d\t%03o\t%02X\t", b, b, b);
        print_binary(b);
        printf("\n");
    }

    fclose(f);
    return 0;
}
```
