ModArbEx Specification: Module, ArbEx, v-1.0.

Requires: Permission_ArbEx
Related permissions: Permission_ArbEx, Permission_ArbEx_root, Permission_ArbEx_su_[user], Permission_ArbEx_assign_handlers[r,w]
Data [store]: ArbEx
Data [transmitted]: Type, List of functions, Handlers to assign.
Structure:


Function:
A module that supports exectuing arbitrary code on a structured format like DLL or SO on a peer. If the Permission_ArbEx is granted to a peer, that peer may send some code, in an SO or DLL format, or, intermediate code like LLVA, depending on the platform. If that code should be translated to a machine language, it will be done, before. Then, a list of functions to execute will be downloaded.

Sequence:
1. Receiving data on functions to import, and, the optional handlers.
2. Running a full permission check - if the Permission_ArbEx_assign_handlers was not set, prompt user.
3. If accepted by the user, or, if permission check ran fine, download the library. The "handlers" table will be updated.
4. A received list of functions to execute, arguments will be processed with the privileges requested.

21/12/14, L, Rishi, Created this document.

License: Same as the original draft of the specification, p2p-dsn, v-1.0