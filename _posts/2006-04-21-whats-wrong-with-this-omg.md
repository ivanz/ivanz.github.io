---
title: 'What&#8217;s wrong with this code? OMG!!!'
author: Ivan Zlatev
layout: post
permalink: /2006/04/21/whats-wrong-with-this-omg/
views:
  - 45
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1331996
categories:
  - Coding
---
I decided to refactor and finish a very old project of mine, initaly coded in x86 assembly. This time it was in C. It has been a while since the last time I worked on it and after refactoring most of it I did a few tests and guess what&#8230; I got a segmentation fault. One hour long I was digging asm code (DDD power with this ugly AT&#038;T asm syntax) and of course C code (again with DDD ;)). I was searching for a tiny little mistake hidden somewhere . Can you find the &#8220;little&#8221; mistake here? I am so ashamed from not finding such a stupid mistake. It actually came from the old code, where it seems i just left some refactoring in the middle of it and forgot about to actually finish it :P.

<pre></pre>

<pre lang="C">uint32_t plGetPeInfo(pl_file *plFile, pl_peinfo *PeInfo, pl_pointers *Pointers)
{
	char *mz;
	char *peHeader;
	IMAGE_NT_HEADERS* pe;
    
	mz = plFile->buffer;
	peHeader = &#038;mz[ read_int32( &#038;mz[0x3c] ) ];

	if (plCheckPe (plFile) == PL_ERROR) {
	
		return PL_ERROR; 
	}

	unpack_struct( "iiIIIiiiccIIIII", &#038;peHeader[4],
		       (char*) &#038;pe->FileHeader.Machine,
		       (char*) &#038;pe->FileHeader.NumberOfSections,
		       (char*) &#038;pe->FileHeader.TimeDateStamp,
		       (char*) &#038;pe->FileHeader.PointerToSymbolTable,
		       (char*) &#038;pe->FileHeader.NumberOfSymbols,
		       (char*) &#038;pe->FileHeader.SizeOfOptionalHeader,
		       (char*) &#038;pe->FileHeader.Characteristics,
		       (char*) &#038;pe->OptionalHeader.Magic,
		       (char*) &#038;pe->OptionalHeader.MajorLinkerVersion,
		       (char*) &#038;pe->OptionalHeader.MinorLinkerVersion,
		       (char*) &#038;pe->OptionalHeader.SizeOfCode,
		       (char*) &#038;pe->OptionalHeader.SizeOfInitializedData,
		       (char*) &#038;pe->OptionalHeader.SizeOfUninitializedData,
		       (char*) &#038;pe->OptionalHeader.AddressOfEntryPoint,
		       (char*) &#038;pe->OptionalHeader.BaseOfCode
		       );


	unpack_struct( "IIIIiiiiiiIII", &#038;peHeader[0x30], 
		       (char*) &#038;pe->OptionalHeader.BaseOfData,
		       (char*) &#038;pe->OptionalHeader.ImageBase,
		       (char*) &#038;pe->OptionalHeader.SectionAlignment,
		       (char*) &#038;pe->OptionalHeader.FileAlignment,
		       (char*) &#038;pe->OptionalHeader.MajorOperatingSystemVersion,
		       (char*) &#038;pe->OptionalHeader.MinorOperatingSystemVersion,
		       (char*) &#038;pe->OptionalHeader.MajorImageVersion,
		       (char*) &#038;pe->OptionalHeader.MinorImageVersion,
		       (char*) &#038;pe->OptionalHeader.MajorSubsystemVersion,
		       (char*) &#038;pe->OptionalHeader.MinorSubsystemVersion,
		       (char*) &#038;pe->OptionalHeader.Reserved1,
		       (char*) &#038;pe->OptionalHeader.SizeOfImage,
		       (char*) &#038;pe->OptionalHeader.SizeOfHeaders
		       );
								
	unpack_struct( "IiiIIIIII", &#038;peHeader[0x58],
		       (char*) &#038;pe->OptionalHeader.CheckSum,
		       (char*) &#038;pe->OptionalHeader.Subsystem,
		       (char*) &#038;pe->OptionalHeader.DllCharacteristics,
		       (char*) &#038;pe->OptionalHeader.SizeOfStackReserve,
		       (char*) &#038;pe->OptionalHeader.SizeOfStackCommit,
		       (char*) &#038;pe->OptionalHeader.SizeOfHeapReserve,
		       (char*) &#038;pe->OptionalHeader.SizeOfHeapCommit,
		       (char*) &#038;pe->OptionalHeader.LoaderFlags,
		       (char*) &#038;pe->OptionalHeader.NumberOfRvaAndSizes	
		       );
	
	 
	if(PeInfo != NULL) {
		PeInfo->EntryPoint = pe->OptionalHeader.AddressOfEntryPoint;
		PeInfo->ImageBase = pe->OptionalHeader.ImageBase;
		PeInfo->SizeOfImage = pe->OptionalHeader.SizeOfImage;
		PeInfo->SizeOfHeaders = pe->OptionalHeader.SizeOfHeaders;
		PeInfo->FileAlignment = pe->OptionalHeader.FileAlignment;
		PeInfo->SizeOfCode = pe->OptionalHeader.SizeOfCode;
		PeInfo->CheckSum = pe->OptionalHeader.CheckSum;
		PeInfo->SectionAlignment = pe->OptionalHeader.SectionAlignment;
		PeInfo->NumberOfSections = pe->FileHeader.NumberOfSections;
	}
 
	if (Pointers != NULL) {
		Pointers->MZHeader = mz;
		Pointers->PeHeader =  peHeader;
		Pointers->SectionsStart = &#038;Pointers->PeHeader[0xf8];
		Pointers->OptionalHeader = &#038;Pointers->PeHeader[0x18];
		Pointers->DirectoriesStart = &#038;Pointers->PeHeader[0x78];
	}
		 
	return PL_SUCCESS;
}

</pre>