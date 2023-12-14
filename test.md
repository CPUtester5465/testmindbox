# Use a Windows base image
FROM mcr.microsoft.com/windows/servercore:ltsc2019

# Copy the .exe files into the container
COPY path/to/your/installer.exe C:/installer.exe

# Run the installation of the program (example)
RUN C:/installer.exe /S

# Enable RDP services and configure the firewall (example commands)
RUN powershell -Command \
    Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -value 0; \
    Enable-NetFirewallRule -DisplayGroup "Remote Desktop"; \
    Set-NetFirewallRule -Name "RemoteDesktop-UserMode-In-TCP" -Enabled True; \
    Set-NetFirewallRule -Name "RemoteDesktop-UserMode-In-UDP" -Enabled True

# Set the password for the Administrator account  !!!DONT FORGET PASSWORD DETETE FILE AFTER INSTALL!!!
RUN net user Administrator "<PASSWORD>"

# Expose the RDP port
EXPOSE 3389

# Indicate a mount point at /data
VOLUME C:/data

# Run an infinite loop to keep the container running
CMD ["powershell", "-Command", "while ($true) { Start-Sleep -Seconds 3600 }"]
