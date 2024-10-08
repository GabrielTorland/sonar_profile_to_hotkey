# SteelSeries Sonar Profile to Hotkey

1. Install AutoHotKeyV2 (https://www.autohotkey.com/v2/) and Wireshark (https://www.wireshark.org/#downloadLink).
2. In Wireshark, capture `Adapter for loopback traffic capture`(localhost).
3. Only consider PUT requests by applying following filter: `http.request.method == "PUT"`.
4. Change to a new profile in SteelSeries GG and then look for a new entry in Wireshark.
5. In the new entry, expand the Hypertext Transfer Protocol and copy the full request URL.
6. You now know the port the API is using as well as the id of profile you just selected.
7. Below is a simple powershell script I created that lets you change profile. Feel free to use it or create your own.
```ps
param (
    [string]$uri
)

$response = Invoke-RestMethod -Uri $uri -Method Put

$profileName = $response.name

# Define a simple popup message box using Add-Type
Add-Type -AssemblyName PresentationFramework

# Display the popup with the selected profile name
[System.Windows.MessageBox]::Show("You have selected the profile: $profileName")
```
8. Lastly, you can bind the script to a keyboard shortcut with the following ahk script.
```ahk
^!+m::Run 'Powershell.exe "C:\path\to\script\select_profile.ps1" "http://localhost:49698/configs/example_id/select"'
```