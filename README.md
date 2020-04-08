# Migration from Series 7

This repository contains our team's collection of documentation and utilities for migrating customers from IBM Cognos Series 7 to IBM Cognos Business Intelligence v10 with the intent being to then upgrade to IBM Cognos Analtyics v11.

## Readiness steps

## Internal Environment
Since the utilities are no longer supported or maintained it is very important not to share them with the customer for security and vulnerability reasons. Therefore we must implement an internal environment to load the customer content into and execute the migration utilities.

There are several steps to implement an internal environment on a Windows 10 virtual machine.

1. Install [Cognos Series 7 Impromptu Administrator](install_impromptua.md)
2. Install [Cognos Business Intelligence v10.1.1](install_bi.md)
3. Install [Migration Tools](install_mig.md)

## Migration Process
TBD

[Migration Example](sample_mig.md)