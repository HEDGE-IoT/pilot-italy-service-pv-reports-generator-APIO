# Reports Generator – Plants Performances Reports

## Services Functional Specifications

---

### General Information and Purpose
- **Service Name/Title**: Reports Generator
- **Description and purpose**: Build and manage Plants performances reports
- **Owner/Contact Information**: Mattia Alfieri (Apio) <m.alfieri@apio.cc>

---

### Functional Requirements
1. Reports generation: The system should be able to generate PV plant performances reports.
2. User Authentication and Authorization: The system should be able to protect resources and functionalities with authorization and authentication.
3. Audit Logging: The system should be able to track configuration modifications.

---

### Non-Functional Requirements
- **Performance**:
  - Report generation for single plants should be completed in < 60 seconds.
  - Report generation for multiple plants should still be completed in < 5 minutes.
- **Reliability and Availability**:
  - The system should provide high availability (targeting 99%) and failover mechanisms.
- **Security (authentication, authorisation, data encryption, data privacy)**:
  - Authentication: Use Token based auth for both interactive and non-interactive auth methods.
  - Authorization: Use RBAC.
  - Encryption: both data at rest and in motion should be encrypted.
- **Privacy**: The system should be compliant with data privacy regulations.

---

### Service Interfaces
Resources are modelled after a REST paradigm. A more complete OpenAPI is available, with parameters, status codes, payloads and responses.

| Method | Path | Description |
|--------|------|-------------|
| GET    | `/projects/{projectId}/plants/{uuid}/reports` | Get the list of available reports for the given Plant |
| POST   | `/projects/{projectId}/plants/{uuid}/reports` | Generate a new report for the given plant or for a group of plants |

---

### Data Model
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Report",
  "type": "object",
  "properties": {
    "type": { "type": "string", "enum": ["pv", "energyCommunity"] },
    "plantId": { "type": "array", "items": { "type": "string" } },
    "plantType": { "type": "string" },
    "projectId": { "type": "string" },
    "plantStatus": { "type": "string" },
    "fileGenerationStatus": { "type": "string" },
    "month": { "type": "number" },
    "year": { "type": "number" },
    "name": { "type": "string" },
    "uuid": { "type": "string" },
    "fileAccessToken": { "type": "string" },
    "createdAt": { "type": "string" },
    "updatedAt": { "type": "string" },
    "fileUrl": { "type": "string" },
    "metadata": { "type": "object", "properties": {}, "required": [] },
    "notes": { "type": "object", "properties": {}, "required": [] }
  },
  "required": [
    "type", "plantId", "plantType", "projectId", "plantStatus",
    "fileGenerationStatus", "month", "year", "name", "uuid",
    "fileAccessToken", "createdAt", "updatedAt", "fileUrl",
    "metadata", "notes"
  ]
}
```

---

### Integration and Dependencies
- **External dependencies**:
- **System dependencies**:
  - IoT Platform: The service is dependent on Device Data and Device Catalogue.
  - Databases, libraries, frameworks and other internal tools.
  - PDF Generation Library.
  - Object Storage system (i.e. AWS S3, MinIO) for report file storage.
- **Third party integrations**: Solar irradiation API for plants where irradiation collecting devices are not available.
- **HEDGE-IOT Edge Devices/interfaces and cloud services integration**.
- **Integration with the App Store**: API-related integration constraints.
- **Integration with the data space**: TBD.

---

### Security and Privacy
- **Data Sensitivity**: TBD
- **Access Control**: Usage of the APIs or of the Frontend requires authentication. Authenticated users may have access to a subset of features depending on the role assigned to them and the permissions assigned to the role.
- **Audit Logs**: user actions that modify the system trigger audit log collection.

---

### Performance Considerations
- **Stress testing**: stress tests are performed periodically.
- **Response times**: Response times are monitored, see 1.9.

---

### Deployment and Environment
- **Configuration**: environment specific settings.
- **CI/CD**: The service is deployed through a Continuous Integration / Continuous Deployment pipeline with the following stages and steps:
  - **Test Stage**
    - Audit: Check for known vulnerabilities at application and container level.
    - Lint: Check for linting errors.
    - Test: Run test suite.
  - **Build Stage**
    - Build Container image.
    - Publish Container image at given registry.
- **Monitoring and Logging**:
  - The service is instrumented with Prometheus Metrics for tracking vitals (resource consumption, application specific metrics).
  - The service is instrumented with a JSON logger.
  - The service is instrumented with Open-telemetry compatible traces.

---

### Testing and Validation
- All commits are checked against a test suite.
