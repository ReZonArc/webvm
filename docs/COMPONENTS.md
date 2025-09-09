# WebVM Component Reference Documentation

This document provides detailed technical information about WebVM's key components and their interactions.

## Core Components

### WebVM.svelte - Main Application Component

The primary Svelte component that orchestrates the entire WebVM experience.

```mermaid
graph TB
    subgraph "WebVM Component Lifecycle"
        Mount[onMount]
        Init[Initialize CheerpX]
        Terminal[Setup Terminal]
        Config[Load Configuration]
        Ready[VM Ready State]
    end
    
    subgraph "Event Handlers"
        Input[Terminal Input]
        Resize[Window Resize] 
        Activity[Activity Monitoring]
        Error[Error Handling]
    end
    
    subgraph "State Management"
        VMState[VM State]
        ProcessCount[Process Counter]
        DiskActivity[Disk I/O Activity]
        CPUActivity[CPU Activity]
    end
    
    Mount --> Init
    Init --> Config
    Config --> Terminal
    Terminal --> Ready
    Ready --> Input
    Ready --> Resize
    Ready --> Activity
    Activity --> VMState
    Activity --> ProcessCount
    Activity --> DiskActivity
    Activity --> CPUActivity
    Input --> Error
    Resize --> Error
```

**Key Properties:**
- `configObj`: Configuration object for VM setup
- `processCallback`: Callback for process monitoring
- `cacheId`: Unique identifier for disk caching
- `cpuActivityEvents`: Array tracking CPU activity
- `diskLatencies`: Array tracking disk I/O performance

**Key Methods:**
- `writeData()`: Sends data to terminal
- `readData()`: Receives input from terminal  
- `printMessage()`: Displays system messages
- `registerActivity()`: Records system activity

### SideBar.svelte - Control Panel Interface

Provides the primary user interface for WebVM controls and information panels.

```mermaid
graph LR
    subgraph "Sidebar Structure"
        Icons[Control Icons]
        Panels[Information Panels]
        State[Panel State Management]
    end
    
    subgraph "Available Panels"
        CPU[CPU Monitor]
        Disk[Disk Monitor]
        Network[Networking]
        AI[Anthropic AI]
        Info[System Information]
        Posts[Blog Posts]
        GitHub[GitHub Integration]
        Discord[Discord Community]
    end
    
    Icons --> CPU
    Icons --> Disk  
    Icons --> Network
    Icons --> AI
    Icons --> Info
    Icons --> Posts
    Icons --> GitHub
    Icons --> Discord
    
    Panels --> State
    State --> Icons
```

**Features:**
- Collapsible panel system
- Real-time activity indicators
- Mouse hover interactions
- Pin/unpin functionality for persistent display

### Terminal Integration (XTerm.js)

WebVM uses XTerm.js for terminal emulation with custom enhancements.

```mermaid
graph TB
    subgraph "Terminal Setup"
        XTerm[XTerm.js Instance]
        FitAddon[Fit Addon]
        WebLinks[Web Links Addon]
        Config[Terminal Config]
    end
    
    subgraph "I/O Pipeline"
        UserInput[User Input]
        InputBuffer[Input Buffer]
        CheerpX[CheerpX Processing]
        Output[Terminal Output]
    end
    
    subgraph "Display Features"
        Cursor[Cursor Management]
        Colors[Color Support]
        Fonts[Font Rendering]
        Resize[Dynamic Resizing]
    end
    
    XTerm --> FitAddon
    XTerm --> WebLinks
    XTerm --> Config
    UserInput --> InputBuffer
    InputBuffer --> CheerpX
    CheerpX --> Output
    Output --> Cursor
    Output --> Colors
    Output --> Fonts
    FitAddon --> Resize
```

**Configuration Options:**
```javascript
const termConfig = {
    cursorBlink: true,
    convertEol: true,
    fontFamily: 'monospace',
    fontSize: 14,
    theme: {
        background: '#1a1a1a',
        foreground: '#ffffff'
    }
}
```

## Activity Monitoring System

WebVM includes comprehensive activity monitoring for system performance visualization.

```mermaid
graph TB
    subgraph "Activity Types"
        CPU[CPU Activity]
        Disk[Disk I/O]
        Network[Network Activity]
        Process[Process Events]
    end
    
    subgraph "Data Collection"
        Events[Event Collection]
        Timestamps[Timestamp Recording]
        Metrics[Metric Calculation]
        Aggregation[Data Aggregation]
    end
    
    subgraph "Visualization"
        Indicators[Activity Indicators]
        Graphs[Performance Graphs]
        Alerts[Activity Alerts]
    end
    
    CPU --> Events
    Disk --> Events
    Network --> Events
    Process --> Events
    Events --> Timestamps
    Timestamps --> Metrics
    Metrics --> Aggregation
    Aggregation --> Indicators
    Aggregation --> Graphs
    Aggregation --> Alerts
```

**Activity Data Structure:**
```javascript
const activityEvent = {
    timestamp: Date.now(),
    type: 'cpu'|'disk'|'network'|'process',
    value: number,
    metadata: object
}
```

## Configuration System

WebVM supports multiple configuration profiles for different use cases.

```mermaid
graph LR
    subgraph "Configuration Files"
        Public[config_public_terminal.js]
        GitHub[config_github_terminal.js]
        Alpine[Alpine Config]
        Custom[Custom Configurations]
    end
    
    subgraph "Configuration Parameters"
        DiskImage[Disk Image URL]
        DiskType[Storage Backend Type]
        Command[Initial Command]
        Environment[Environment Variables]
        WorkingDir[Working Directory]
        UserSettings[User/Group Settings]
    end
    
    Public --> DiskImage
    GitHub --> DiskImage
    Alpine --> DiskImage
    Custom --> DiskImage
    
    DiskImage --> DiskType
    DiskType --> Command
    Command --> Environment
    Environment --> WorkingDir
    WorkingDir --> UserSettings
```

**Configuration Example:**
```javascript
export const diskImageUrl = "wss://disks.webvm.io/debian_large.ext2";
export const diskImageType = "cloud";
export const cmd = "/bin/bash";
export const args = ["--login"];
export const opts = {
    env: ["HOME=/home/user", "TERM=xterm"],
    cwd: "/home/user",
    uid: 1000,
    gid: 1000
};
```

## Network Integration

WebVM's networking is built around Tailscale VPN integration for secure connectivity.

```mermaid
sequenceDiagram
    participant User
    participant NetworkTab as Network UI
    participant TailscaleClient as Tailscale Client
    participant TailscaleAPI as Tailscale API
    participant WebVM as WebVM Instance
    
    User->>NetworkTab: Click "Connect to Tailscale"
    NetworkTab->>TailscaleAPI: Initiate auth flow
    TailscaleAPI-->>User: Display login page
    User->>TailscaleAPI: Complete authentication
    TailscaleAPI-->>NetworkTab: Auth success
    NetworkTab->>TailscaleClient: Establish connection
    TailscaleClient->>WebVM: Configure network interface
    WebVM-->>User: Network connectivity ready
```

**Network Features:**
- WebSocket-based VPN connectivity
- Peer-to-peer networking when possible
- DERP relay fallback for NAT traversal
- End-to-end encryption
- DNS resolution through Tailscale

## AI Integration (Anthropic Claude)

WebVM includes integrated AI assistance through Anthropic's Claude API.

```mermaid
graph TB
    subgraph "AI Interface"
        AnthropicTab[Anthropic Tab UI]
        APIKey[API Key Management]
        ChatInterface[Chat Interface]
        ToolIntegration[Tool Integration]
    end
    
    subgraph "Message Processing"
        InputProcessing[Input Processing]
        ContextManagement[Context Management]
        ResponseHandling[Response Handling]
        ToolExecution[Tool Execution]
    end
    
    subgraph "Claude API"
        MessageAPI[Messages API]
        TokenManagement[Token Management]
        ErrorHandling[Error Handling]
        RateLimit[Rate Limiting]
    end
    
    AnthropicTab --> APIKey
    APIKey --> ChatInterface
    ChatInterface --> InputProcessing
    InputProcessing --> ContextManagement
    ContextManagement --> MessageAPI
    MessageAPI --> TokenManagement
    MessageAPI --> ResponseHandling
    ResponseHandling --> ToolExecution
    ToolExecution --> ToolIntegration
```

**AI Features:**
- Interactive chat interface
- Command execution assistance
- Code analysis and debugging
- System troubleshooting
- Educational explanations

## File System Architecture

WebVM implements a sophisticated virtual file system with cloud-based storage.

```mermaid
graph TB
    subgraph "File System Layers"
        VFS[Virtual File System]
        BlockLayer[Block Layer]
        Cache[Block Cache]
        Network[Network Layer]
    end
    
    subgraph "Storage Backends"
        CloudDisk[Cloud Disk Images]
        LocalCache[Browser Local Storage]
        MemoryCache[In-Memory Cache]
    end
    
    subgraph "File Operations"
        Read[File Read]
        Write[File Write]
        Directory[Directory Operations]
        Metadata[Metadata Operations]
    end
    
    VFS --> BlockLayer
    BlockLayer --> Cache
    Cache --> Network
    Network --> CloudDisk
    Cache --> LocalCache
    Cache --> MemoryCache
    
    Read --> VFS
    Write --> VFS
    Directory --> VFS
    Metadata --> VFS
```

**File System Features:**
- Ext2 file system support
- Copy-on-write semantics
- Automatic block caching
- Lazy loading of disk blocks
- Persistent local caching

## Build System and Deployment

WebVM uses a sophisticated build pipeline for creating and deploying disk images.

```mermaid
graph LR
    subgraph "Source"
        Dockerfile[Dockerfile]
        Assets[Static Assets]
        Config[Configuration]
        Code[Source Code]
    end
    
    subgraph "Build Pipeline"
        DockerBuild[Docker Build]
        FSExtract[Filesystem Extract]
        Ext2Convert[Ext2 Conversion]
        Compress[Compression]
        Upload[Upload to CDN]
    end
    
    subgraph "Deployment"
        GitHubPages[GitHub Pages]
        DiskServer[Disk Storage Server]
        CDN[Content Delivery Network]
    end
    
    Dockerfile --> DockerBuild
    Assets --> DockerBuild
    Config --> DockerBuild
    Code --> DockerBuild
    DockerBuild --> FSExtract
    FSExtract --> Ext2Convert
    Ext2Convert --> Compress
    Compress --> Upload
    Upload --> DiskServer
    Code --> GitHubPages
    GitHubPages --> CDN
```

**Build Process Features:**
- Automated Docker image building
- Filesystem extraction and conversion
- Optimized disk image compression
- Automated deployment to CDN
- Version management and rollback

## Performance Monitoring

WebVM includes comprehensive performance monitoring capabilities.

```mermaid
graph TB
    subgraph "Metrics Collection"
        CPUMetrics[CPU Usage Metrics]
        MemoryMetrics[Memory Usage Metrics] 
        DiskMetrics[Disk I/O Metrics]
        NetworkMetrics[Network Metrics]
    end
    
    subgraph "Data Processing"
        Aggregation[Data Aggregation]
        Smoothing[Data Smoothing]
        Alerting[Alert Generation]
        Storage[Metric Storage]
    end
    
    subgraph "Visualization"
        RealTime[Real-time Indicators]
        Charts[Performance Charts]
        Dashboard[Performance Dashboard]
        Export[Data Export]
    end
    
    CPUMetrics --> Aggregation
    MemoryMetrics --> Aggregation
    DiskMetrics --> Aggregation
    NetworkMetrics --> Aggregation
    Aggregation --> Smoothing
    Smoothing --> Alerting
    Alerting --> Storage
    Storage --> RealTime
    Storage --> Charts
    Storage --> Dashboard
    Storage --> Export
```

**Performance Features:**
- Real-time performance monitoring
- Historical performance data
- Performance alerts and notifications
- Exportable performance metrics
- Customizable monitoring dashboards

This component reference provides the technical foundation for understanding, extending, and maintaining WebVM's architecture.