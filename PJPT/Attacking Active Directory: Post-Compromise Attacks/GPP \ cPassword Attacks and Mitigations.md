## GPP / cPassword Attacks and Mitigations
- Older attack, almost 10 years old
- Group Policy Preferences (GPP) allowed admins to create policies using embedded credentials
- These credentials were encrypted and placed in a "cPassword"
- The key was accidentally released (whoops)
- Patched in MS14-025, but it doesn't prevent previous uses
- If the DC is not patched or if previous uses were there, it still exists
- STILL RELEVANT ON PENTESTS

### Attacks
- Can use `gpp-decrypt <cpassword>` to decrypt it
- Can use metasploit `auxiliary/scanner/smb/smb_enum_gpp` using credentials to access SYSVOL

### Mitigations
- PATCH! Fixed in KB2962486
- In reality: delete the old GPP xml files stored in the SYSVOL
