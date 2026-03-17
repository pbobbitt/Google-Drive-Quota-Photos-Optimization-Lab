# Storage Optimization Command Reference

## Data Integrity Verification
To verify the SHA-256 hash of a downloaded archive:
`Get-FileHash "C:\Path\To\File.zip"`

## Batch Hashing (Optional/Advanced)
To hash all zip files in a folder at once:
`Get-ChildItem -Filter *.zip | Get-FileHash`
