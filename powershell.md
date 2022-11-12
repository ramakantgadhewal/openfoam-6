 --remove the first few characters from multiple files at once in Windows PowerShell
    Removes first 5 chars from all *.txt files
    get-childitem *.txt | rename-item -newname { [string]($_.name).substring(5) }