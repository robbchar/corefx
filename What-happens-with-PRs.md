The following is describing the process for handling PRs  

1. The triage group or owner will make sure it is tagged correctly.

2. The owner will work with the submitter to make sure the bug fix is correct and complete.

3. Additionally, the owner will make sure that:

 -	The fix preserves back-compat (Using if-defs around the code or using the mechanisms defined in the AppContext class)
 - Any new APIs have been reviewed and approved.
 - The change follows the coding guidelines

4. The owner will evaluate the possibility of porting the change to CoreCLR or Desktop and will follow the appropriate process.

5. Once the owner signs off and at least one other contributor signs off then the owner is free to merge in the change
