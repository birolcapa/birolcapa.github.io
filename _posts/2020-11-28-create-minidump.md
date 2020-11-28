---
layout: post
title:  "Creating Minidump File Without a Crash"
date:   2020-11-28 15:41:00 +0200
categories: software
---

Let's imagine that a windows cpp application crashes intermittently.  
In this case a minidump file which can show me the callstack would be really helpful.  
I was lucky because some people already had a similar problem:

What is try-except statement:  
<https://docs.microsoft.com/en-us/cpp/cpp/try-except-statement?view=msvc-160>  

How to create a minidump file:  
<http://ntcoder.com/bab/2014/10/14/how-to-create-full-memory-dumps-using-minidumpwritedump/>  

How to use try_except statement:  
<https://stackoverflow.com/questions/7049502/c-try-and-try-catch-finally>  
<https://stackoverflow.com/questions/43193901/is-it-possible-to-create-mini-dump-file-programmatically-without-a-crash>  

All I need to do combine the suggestions and create a solution which can help me.  
Here is the MiniDumpFileGenerator class which can generate minidump file without a crash.

```cpp
#pragma once

#include <iostream>
#include <string>
#include <windows.h>
#include <DbgHelp.h>

#pragma comment (lib, "dbghelp.lib")

class MiniDumpFileGenerator
{
public:
    static void GenerateMiniDumpFile(std::string fileName);

private:
    static std::string sFileName;

    static void GenerateMiniDumpFileUsingAccessViolationException();
    static int FilterExceptionAndContinueExecution(
        EXCEPTION_POINTERS* exceptionPointers);
    static void WriteMiniDumpFile(EXCEPTION_POINTERS* exceptionPointers);
    static HANDLE CreateMiniDumpFile();
};
```

```cpp
#include "MiniDumpFileGenerator.h"

std::string MiniDumpFileGenerator::sFileName = "";

void MiniDumpFileGenerator::GenerateMiniDumpFile(std::string fileName)
{
    sFileName = fileName;
    GenerateMiniDumpFileUsingAccessViolationException();
}

void MiniDumpFileGenerator::GenerateMiniDumpFileUsingAccessViolationException()
{
    __try
    {
        int* createException = nullptr;
        *createException = 0x42;
    }
    __except (FilterExceptionAndContinueExecution(GetExceptionInformation()))
    {
        // Use filter exception to generate minidump with the "exception record"
        // Then continue execution
    }
}

int MiniDumpFileGenerator::FilterExceptionAndContinueExecution(
    EXCEPTION_POINTERS * exceptionPointers)
{
    WriteMiniDumpFile(exceptionPointers);
    // With the following return statement
    // Execution continues after intentionally created access violation exception
    return EXCEPTION_EXECUTE_HANDLER;
}

void MiniDumpFileGenerator::WriteMiniDumpFile(
    EXCEPTION_POINTERS * exceptionPointers)
{
    HANDLE hFile = CreateMiniDumpFile();
    if (!hFile)
    {
        std::cout << "Failed to create dump file" << std::endl;
    }
    else
    {
        MINIDUMP_EXCEPTION_INFORMATION minidumpExceptionInformation;
        minidumpExceptionInformation.ThreadId = GetCurrentThreadId();
        minidumpExceptionInformation.ExceptionPointers = exceptionPointers;
        minidumpExceptionInformation.ClientPointers = TRUE;

        bool isMiniDumpGenerated = MiniDumpWriteDump(
            GetCurrentProcess(),
            GetCurrentProcessId(),
            hFile,
            MINIDUMP_TYPE::MiniDumpNormal,
            &minidumpExceptionInformation,
            nullptr,
            nullptr);

        CloseHandle(hFile);

        if (!isMiniDumpGenerated)
        {
            std::cout << "MiniDumpWriteDump failed" << std::endl;
        }
    }
}

HANDLE MiniDumpFileGenerator::CreateMiniDumpFile()
{
    return CreateFile(sFileName.c_str(),
        GENERIC_ALL, 0, nullptr, CREATE_ALWAYS, FILE_ATTRIBUTE_NORMAL, nullptr);
}

```
Here is a sample:  

```cpp
void SuspiciousMethod()
{
    // Very important and complicated business logic here
    std::cout << __func__ << ": Begin" << std::endl;
    // Some complicated business logic here
    std::cout << __func__ << ": Before minidump file generation" << std::endl;
    MiniDumpFileGenerator::GenerateMiniDumpFile("C:\\WhenFalse.dmp");
    std::cout << __func__ << ": After minidump file generation" << std::endl;
    // More important complicated business logic there
    std::cout << __func__ << ": End" << std::endl;
}

int main()
{
    std::cout << __func__ << ": Before SuspiciousMethod()" << std::endl;
    SuspiciousMethod();
    std::cout << __func__ << ": After SuspiciousMethod()" << std::endl;
    return 0;
}
```

Finally, here is the output:  
![Program Output](/images/Minidumpsamleusage.png)  

Output shows me that execution continues after intentionally created access violation exception.  
I also end up with a nice call stack in which I can see what may cause the intermittent crash:  

![Call Stack](/images/Minidumpsamleusage_callstack.png)

