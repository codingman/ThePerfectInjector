# ThePerfectInjector
Literally, the perfect injector.

Detailed explanation: https://blog.can.ac/2018/05/02/making-the-perfect-injector-abusing-windows-address-sanitation-and-cow/

1、Load vulnerable driver

2、Map physical memory to user-mode

3、Search for certain offsets (UniqueProcessId, DirectoryTableBase, ActiveProcessLinks)

4、Save current EProcess and CR3 values for user-mode use

5、Allocate enough kernel pool memory for our injector stub and image

6、Unload vulnerable driver

7、Map our image to the kernel memory (Fix .relocs and create a stub that gets the imports for us as I cannot bother reading EProcess->Peb)

8、Wait for target process

9、Expose the kernel page to target process

10、Hook TlsGetValue system-wide and make it check for pid before jumping to our stub at kernel memory

11、Wait for Stub->SpinningThreadCount to be non zero

12、Unhook TlsGetValue, set Stub->Free = TRUE

13、Profit.

