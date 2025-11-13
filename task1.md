'''
graph LR
    A[Start] --> B[Read CSV & DNS Config]
    B --> C[Connect to Device]
    C --> D[Backup Current Config]
    D --> E[Add New DNS Servers]
    E --> F[Verify New DNS Active]
    F --> G[Remove Old DNS Servers]
    G --> H[Save Config]
    H --> I[Log Results]
    I --> J{More Devices?}
    J -->|Yes| C
    J -->|No| K[Generate Report]
```

## Core Implementation

### Main Script Structure
1. Load device list from CSV
2. Load new DNS servers from config file
3. For each device:
   - Connect and identify current DNS servers
   - Add new DNS servers
   - Verify connectivity (optional ping test)
   - Remove old DNS servers
   - Save configuration
4. Generate summary report

### Essential Error Handling
- **Can't connect** → Log error, skip device, continue
- **Auth failed** → Log error, skip device, continue  
- **Command failed** → Log error, rollback changes if possible, skip device
- **All DNS removal would fail** → Keep at least one DNS server, alert user

## File Structure (Minimal)
```
dns-update/
├── update_dns.py           # Main script
├── devices.csv             # Device inventory
├── dns_config.yml          # New DNS servers to add
├── .env                    # Credentials
├── output/                 # Results directory
│   └── results_YYYYMMDD_HHMMSS.json
└── requirements.txt        # Python dependencies
