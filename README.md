# VLAN-Configuration-and-Inter-VLAN-Routing

### **VLAN (Virtual Local Area Network) Configuration and Inter-VLAN Routing**

#### 1. **VLAN Overview**:
   - **Definition**: VLAN is a logical grouping of devices within a LAN, allowing segmentation of network traffic into different broadcast domains.
   - **Purpose**: Improves security, reduces broadcast traffic, and simplifies network management.

#### 2. **VLAN Configuration**:
   - **Creating VLANs**: VLANs are created on network devices like switches to segregate network traffic.
   - **VLAN ID**: Each VLAN is identified by a unique number (1-4095, typically 1-1005 for standard VLANs).
   - **Assigning VLANs to Ports**: Switch ports can be assigned to specific VLANs to ensure devices in the same VLAN can communicate.

#### 3. **VLAN Configuration Example (Cisco Switch)**:
   ```shell
   vlan 10
     name Sales
   vlan 20
     name HR
   ```
   - **Assigning Ports to VLAN**:
     ```shell
     interface range fa0/1 - 10
     switchport mode access
     switchport access vlan 10
     ```
     - This configures ports `fa0/1` to `fa0/10` to belong to VLAN 10 (Sales).

#### 4. **Trunking (VLAN Tagging)**:
   - **Purpose**: Allows multiple VLANs to traverse a single link between switches or between a switch and a router.
   - **802.1Q Tagging**: The standard used for VLAN tagging, where the VLAN ID is added to Ethernet frames to identify the VLAN.
   - **Trunk Port Configuration**:
     ```shell
     interface fa0/24
     switchport mode trunk
     switchport trunk allowed vlan 10,20
     ```

#### 5. **Inter-VLAN Routing (Layer 3 Routing)**:
   - **Definition**: Allows devices in different VLANs to communicate with each other.
   - **Router-on-a-Stick**: A method of routing traffic between VLANs using a single router interface and subinterfaces for each VLAN.
   - **Configuration**:
     - **Create Subinterfaces on Router**:
       ```shell
       interface gig0/0.10
       encapsulation dot1Q 10
       ip address 192.168.10.1 255.255.255.0
       
       interface gig0/0.20
       encapsulation dot1Q 20
       ip address 192.168.20.1 255.255.255.0
       ```
     - **Router Subinterface Configuration**:
       - Each subinterface is configured with the VLAN ID (using `encapsulation dot1Q <VLAN ID>`) and an IP address.
       - This allows the router to route between VLAN 10 and VLAN 20.

#### 6. **VLAN Routing Alternatives**:
   - **Layer 3 Switches**: A Layer 3 switch can perform routing between VLANs internally without a dedicated router.
     - **SVI (Switched Virtual Interface)**:
       ```shell
       interface vlan 10
       ip address 192.168.10.1 255.255.255.0
       no shutdown
       ```
     - The switch performs routing for the VLAN internally, similar to a router.

#### 7. **VLAN Best Practices**:
   - **Limit Broadcast Domains**: Keep VLANs small to limit broadcast traffic and improve performance.
   - **Use Descriptive VLAN Names**: Use meaningful names (e.g., Sales, HR) for easier management.
   - **Security Considerations**: Use VLANs to isolate sensitive devices and minimize unauthorized access.
   - **VLAN Pruning**: Disable VLANs on trunk links that are not required, reducing unnecessary traffic.

#### 8. **VLAN Troubleshooting**:
   - Verify VLAN configuration on switches using `show vlan brief`.
   - Check trunk configuration using `show interfaces trunk`.
   - Verify routing between VLANs using `ping` and `traceroute`.

### **Summary**:
- **VLAN**: Creates logical segments in a network.
- **Trunking**: Carries multiple VLANs over a single physical link.
- **Inter-VLAN Routing**: Enables communication between different VLANs using a router or Layer 3 switch.
- Proper configuration and segmentation improve network efficiency, security, and manageability.
