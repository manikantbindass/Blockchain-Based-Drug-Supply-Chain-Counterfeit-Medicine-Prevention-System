# Smart Contract Documentation

This folder documents the design and implementation of blockchain smart contracts (chaincode) for the drug supply chain system.

## Purpose

Provides comprehensive documentation of Hyperledger Fabric chaincode modules that enforce business logic, access control, and data integrity rules directly on the blockchain ledger.

## Chaincode Modules

### Batch Management Chaincode
**Purpose**: Core batch lifecycle management on blockchain

**Responsibilities**:
- Register new medicine batches with complete metadata
- Record manufacturing details and quality certifications
- Generate cryptographically secure batch identifiers
- Set expiry dates and storage requirements
- Manage batch status transitions throughout lifecycle
- Validate batch data integrity

**Key Functions**:
- `CreateBatch(batchData)` - Register new batch on ledger
- `GetBatch(batchId)` - Retrieve complete batch details
- `UpdateBatchStatus(batchId, status)` - Change batch lifecycle status
- `QueryBatchesByManufacturer(manufacturerId)` - Search by manufacturer
- `GetBatchHistory(batchId)` - Complete audit trail

### Transfer Chaincode
**Purpose**: Custody chain management between supply chain participants

**Responsibilities**:
- Record ownership transfers with cryptographic signatures
- Validate transfer authorization and permissions
- Enforce custody chain business rules
- Maintain immutable transfer history
- Prevent unauthorized or fraudulent transfers
- Track location and timestamp data

**Key Functions**:
- `InitiateTransfer(batchId, fromParty, toParty)` - Start transfer
- `AcceptTransfer(transferId, signature)` - Accept custody with signature
- `RejectTransfer(transferId, reason)` - Reject with explanation
- `GetTransferHistory(batchId)` - Complete custody chain
- `QueryPendingTransfers(orgId)` - List pending actions

### Recall Chaincode
**Purpose**: Product recall management and enforcement

**Responsibilities**:
- Initiate batch recalls by authorized regulators
- Freeze affected batches preventing further transfers
- Notify all current custody holders
- Track recall completion status
- Generate compliance reports
- Maintain recall audit trail

**Key Functions**:
- `InitiateRecall(batchId, reason, regulator)` - Start recall
- `FreezeBatch(batchId)` - Prevent all transfers
- `ConfirmRecallCompletion(batchId, holder)` - Mark batch returned
- `GetRecallStatus(batchId)` - Check progress
- `ListActiveRecalls()` - All active recalls

### Audit Chaincode
**Purpose**: Regulatory audit and compliance queries

**Responsibilities**:
- Provide regulator-specific query functions
- Generate comprehensive audit reports
- Track compliance violations
- Maintain immutable audit logs
- Support regulatory investigations
- Enable real-time compliance monitoring

**Key Functions**:
- `AuditBatch(batchId)` - Complete batch audit trail
- `QueryViolations(criteria)` - Find compliance issues
- `GenerateComplianceReport(dateRange)` - Regulatory report
- `TrackBatchLocation(batchId)` - Current location
- `GetSupplyChainMetrics()` - Network-wide statistics

### Access Control Chaincode
**Purpose**: On-chain role and permission enforcement

**Responsibilities**:
- Define role-based access policies
- Enforce permissions at chaincode level
- Validate caller identity and attributes
- Implement multi-signature requirements
- Audit access attempts

**Key Functions**:
- `CheckPermission(caller, action, resource)` - Permission check
- `GetRolePermissions(role)` - List role capabilities
- `ValidateSignatures(signatures, threshold)` - Multi-sig validation

## Chaincode Governance

### Deployment Process
1. Code review by security team
2. Unit and integration testing
3. Testnet deployment and validation
4. Endorsement policy definition
5. Production deployment with versioning
6. Post-deployment monitoring

### Upgrade Strategy
- Semantic versioning for chaincode
- Backward compatibility for data structures
- Migration scripts for state transitions
- Phased rollout across organizations
- Rollback procedures for failures

### Endorsement Policies
- Batch creation: Manufacturer signature required
- Transfer: Both sender and receiver signatures
- Recall: Regulator signature required
- Audit: Any authorized regulator

## Data Models

### Batch Structure
```go
type Batch struct {
    BatchID       string
    ProductName   string
    Manufacturer  string
    ManufactureDate time.Time
    ExpiryDate    time.Time
    Quantity      int
    Status        string
    Certifications []string
    CurrentHolder  string
}
```

### Transfer Structure
```go
type Transfer struct {
    TransferID   string
    BatchID      string
    FromParty    string
    ToParty      string
    InitiatedAt  time.Time
    CompletedAt  time.Time
    Status       string
    Signatures   []Signature
}
```

## Security Considerations

- Private data collections for sensitive information
- Encryption of confidential batch details
- Access control via MSP and attributes
- Rate limiting on queries
- Audit logging of all invocations

## How This Fits Into The System

Smart contracts provide:
- Tamper-proof business logic execution
- Distributed consensus on transactions
- Immutable audit trails for compliance
- Cryptographic verification of data
- Decentralized trust without intermediaries

## Current Status

**Status**: Planned

Chaincode modules are documented with function signatures and data models defined. Implementation will be done in Go following Hyperledger Fabric best practices.

Files `contract-design.md` and `access-control-model.md` will provide deeper implementation details.
