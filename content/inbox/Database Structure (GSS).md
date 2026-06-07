```
[users] 
   │ (1)
   └───< [organization_members] >───(1) [organizations]
                │ (1)                       │ (1)
                │                           └───> [folders] (Self-referencing Tree)
                │                                       │ (1)
                └───> [folder_permissions] <────────────┘ (1)
                            │ (1)
                            └───> [projects]
```
