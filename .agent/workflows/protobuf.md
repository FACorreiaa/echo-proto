---
description: How to work with Protocol Buffers and generate code
---

# Protobuf Development Workflow

## Generate Code

### Generate All (Go + TypeScript)
// turbo
1. Generate code for all languages:
```bash
cd /Users/fernando_idwell/Projects/FinanceTrackerEcho/Echo-proto && buf generate
```

### Lint Proto Files
// turbo
2. Lint protobuf definitions:
```bash
cd /Users/fernando_idwell/Projects/FinanceTrackerEcho/Echo-proto && buf lint
```

### Format Proto Files
// turbo
3. Format protobuf files:
```bash
cd /Users/fernando_idwell/Projects/FinanceTrackerEcho/Echo-proto && buf format -w
```

### Check Breaking Changes
4. Check for breaking changes:
```bash
cd /Users/fernando_idwell/Projects/FinanceTrackerEcho/Echo-proto && buf breaking --against '.git#branch=main'
```

## Push to BSR (Buf Schema Registry)

### Push to BSR
5. Push schema to BSR:
```bash
cd /Users/fernando_idwell/Projects/FinanceTrackerEcho/Echo-proto && buf push
```

### Login to BSR
6. Login to Buf Schema Registry:
```bash
buf registry login
```

## Local Development

### Update Dependencies
// turbo
7. Update buf dependencies:
```bash
cd /Users/fernando_idwell/Projects/FinanceTrackerEcho/Echo-proto && buf dep update
```

### Build (Verify Compilation)
// turbo
8. Verify proto files compile:
```bash
cd /Users/fernando_idwell/Projects/FinanceTrackerEcho/Echo-proto && buf build
```

## Project Structure

```
Echo-proto/
├── buf.yaml           # Buf module configuration
├── buf.gen.yaml       # Code generation configuration
├── buf.lock           # Dependency lock file
└── echo/
    └── v1/
        ├── auth.proto       # Authentication service
        ├── balance.proto    # Balance engine
        ├── finance.proto    # Transactions
        ├── goal.proto       # Goals tracking
        ├── insights.proto   # Analytics/insights
        └── plan.proto       # Budgeting plans
```

## Adding a New Service

1. Create new `.proto` file in `echo/v1/`
2. Define your service:
```protobuf
syntax = "proto3";

package echo.v1;

service NewService {
  rpc GetSomething(GetSomethingRequest) returns (GetSomethingResponse) {}
}

message GetSomethingRequest {
  string id = 1;
}

message GetSomethingResponse {
  Something something = 1;
}
```

3. Run `buf lint` to check for issues
4. Run `buf generate` to generate code
5. Push to BSR: `buf push`

## Generated Code Locations

After running `buf generate`:

| Language | Output Location |
|----------|-----------------|
| Go | `gen/go/echo/v1/` |
| TypeScript | Published to BSR, consumed via npm |

## Consuming Generated Types

### In Go (EchoAPI)
```go
import echov1 "buf.build/gen/go/echo-tracker/echo/protocolbuffers/go/echo/v1"
```

### In TypeScript (EchoApp)
```typescript
import { GetBalanceRequest } from "@buf/echo-tracker_echo.bufbuild_es/echo/v1/balance_pb";
```

## Best Practices

- Always run `buf lint` before committing
- Use `buf breaking` to check for breaking changes
- Keep services focused and cohesive
- Use `google.protobuf.Timestamp` for dates
- Use int64 for money (minor units/cents)
- Add comments for documentation
