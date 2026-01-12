# System Design Document Template

Use this template to generate `docs/prd/02-system-design.md`.

```markdown
# System Design: <Feature Name>

## 1. Target Platform

- **Platform**: <Web/iOS/Android/Desktop/Server/Cross-platform>
- **Minimum Version**: <e.g., iOS 14+, Android 8+, Chrome 90+>
- **Deployment**: <Cloud provider / On-premise / Hybrid>

## 2. Architecture Overview

```
<ASCII architecture diagram>

Example:
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Client    │────>│   API GW    │────>│   Service   │
└─────────────┘     └─────────────┘     └─────────────┘
                                              │
                                              v
                                        ┌─────────────┐
                                        │  Database   │
                                        └─────────────┘
```

## 3. Tech Stack

> User Preference: <User specified stack / No preference>

| Category | Choice | Rationale |
|----------|--------|-----------|
| Language | <tech> | <why> |
| Framework | <tech> | <why> |
| Database | <tech> | <why> |
| Cache | <tech> | <why> |
| Message Queue | <tech> | <why> |
| Test Framework | <tech> | <why> |

## 4. Module Design

| Module | Responsibility | Dependencies |
|--------|----------------|--------------|
| <name> | <description> | <other modules> |

### 4.1 <Module Name>

**Responsibility**: <detailed description>

**Key Components**:
- Component A: <description>
- Component B: <description>

## 5. Data Model

### 5.1 Entity Definitions

#### <Entity Name>

| Field | Type | Description | Index |
|-------|------|-------------|-------|
| id | string/uuid | Primary key | PK |
| created_at | timestamp | Creation time | IDX |
| updated_at | timestamp | Update time | - |

### 5.2 Entity Relationships

```
User 1──* Order
Order *──* Product
```

### 5.3 Index Strategy

| Table | Index Name | Columns | Type | Purpose |
|-------|------------|---------|------|---------|
| <table> | <name> | <cols> | <B-tree/Hash> | <query scenario> |

## 6. API Design

### 6.1 <API Name>

- **Method**: `POST`
- **Path**: `/api/v1/resource`
- **Description**: <what it does>
- **Authentication**: <required/optional>

**Request Headers**:
```
Authorization: Bearer <token>
Content-Type: application/json
```

**Request Body**:
```json
{
  "field1": "value1",
  "field2": 123
}
```

**Response (Success)**:
```json
{
  "code": 0,
  "message": "success",
  "data": {
    "id": "xxx",
    "created_at": "2024-01-01T00:00:00Z"
  }
}
```

**Response (Error)**:
```json
{
  "code": 1001,
  "message": "Invalid input",
  "details": {
    "field": "field1",
    "reason": "required"
  }
}
```

## 7. State Flow

### 7.1 <Business Flow Name>

```
┌─────────┐    create    ┌─────────┐    submit    ┌─────────┐
│  Draft  │─────────────>│ Pending │─────────────>│ Active  │
└─────────┘              └─────────┘              └─────────┘
                              │                        │
                              │ cancel                 │ complete
                              v                        v
                         ┌─────────┐              ┌─────────┐
                         │Cancelled│              │Completed│
                         └─────────┘              └─────────┘
```

| From State | Event | To State | Side Effects |
|------------|-------|----------|--------------|
| Draft | create | Pending | Send notification |
| Pending | submit | Active | Update timestamp |

## 8. Error Handling

### 8.1 Error Code Design

| Code Range | Category | Description |
|------------|----------|-------------|
| 1000-1999 | Client Error | Invalid input, authentication |
| 2000-2999 | Business Error | Business rule violations |
| 3000-3999 | System Error | Internal errors |

### 8.2 Error Code List

| Code | Message | Description | Handling Strategy |
|------|---------|-------------|-------------------|
| 1001 | Invalid input | Field validation failed | Return field details |
| 1002 | Unauthorized | Token invalid/expired | Redirect to login |
| 2001 | Resource not found | Entity doesn't exist | Return 404 |
| 3001 | Internal error | Unexpected error | Log and alert |

## 9. Scalability Considerations

### 9.1 Current Design Limits

- Max concurrent users: <number>
- Max requests/second: <number>
- Data retention: <period>

### 9.2 Future Extension Points

- [ ] <Extension point 1>: <description>
- [ ] <Extension point 2>: <description>

### 9.3 Migration Path

<How to migrate if scale requirements change>
```

## Split File Guidelines

When document exceeds 300 lines, split as follows:

### 02-system-design.md (Main)
- Sections 1-4: Platform, Architecture, Tech Stack, Module Design

### 02-system-design-api.md
- Section 6: Complete API Design
- Section 7: State Flow

### 02-system-design-data.md
- Section 5: Data Model (all entities)
- Section 8: Error Handling
- Section 9: Scalability
