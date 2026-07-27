Act as a Principal Java Architect. Generate a production-ready Phase 1 Fee Module for a School Management System using Spring Boot 3.x / Java 21, Gradle, PostgreSQL, Spring Data JPA, and Spring Security.

Follow Clean Architecture, SOLID principles, REST API best practices, and package-by-feature organization under `com.school.fee`.

### Core Requirements & Domain Constraints:
1. Scope (Phase 1): Fee Categories, Fee Structures, Student Fee Assignment, Due/Paid Fees Tracking, Receipt Generation (Manual/Cash/Cheque), Dashboard Stats, Fee Reports.
2. EXCLUDE Phase 2 features: Do NOT implement payment gateways, online checkout, wallets, or refunds.
3. User Roles & Access Control:
   - ADMIN: Full CRUD on Fee Categories, Structures, Assignments, Manual Receipt Generation, and Dashboard Stats.
   - TEACHER: Read-only access to Student Fee Summaries and Outstanding/Paid statuses.
   - PARENT: Read-only access to own child's Fee Summary, Due Fees, Paid Fees, and Receipt lists.
4. Controllers:
   - `AdminFeeController` (`/api/v1/admin/fees`) -> `@PreAuthorize("hasRole('ADMIN')")`
   - `TeacherFeeController` (`/api/v1/teacher/fees`) -> `@PreAuthorize("hasRole('TEACHER')")`
   - `ParentFeeController` (`/api/v1/parent/fees`) -> `@PreAuthorize("hasRole('PARENT')")`
   Business logic must be shared via common service classes (`ParentFeeService`, `ReceiptService`, etc.).

### System Components to Generate:

1. **Entities & Audit Base Class (`com.school.fee.entity`)**:
   - `BaseAuditableEntity` (abstract `@MappedSuperclass` with `createdDate`, `updatedDate`, `createdBy`, `updatedBy`, `@Enumerated EntityStatus status`).
   - `AcademicYear` (`id` UUID, `yearName`, `startDate`, `endDate`).
   - `FeeCategory` (`id` UUID, `name` UNIQUE, `description`).
   - `FeeStructure` (`id` UUID, `category`, `academicYear`, `className`, `amount`, `dueDate`, `frequency`).
   - `FeeAssignment` (`id` UUID, `feeStructure`, `studentId`, `classId`, `assignmentType`).
   - `StudentFee` (`id` UUID, `feeAssignment`, `studentId`, `totalAmount`, `paidAmount`, `dueAmount`, `dueDate`, `feeStatus`).
   - `Receipt` (`id` UUID, `studentFee`, `receiptNumber` UNIQUE, `amountPaid`, `paymentDate`, `paymentMode`, `transactionReference`, `remarks`).
   - Add proper JPA indexes (`@Index`) on frequently queried columns (`studentId`, `dueDate`, `feeStatus`, `receiptNumber`).

2. **DTOs (`com.school.fee.dto`)**:
   - Use Java 21 Records.
   - Include Jakarta Validation annotations (`@NotNull`, `@NotBlank`, `@Positive`, `@PastOrPresent`, `@FutureOrPresent`).
   - Request DTOs: `FeeCategoryRequest`, `FeeStructureRequest`, `AssignFeeToStudentRequest`, `CreateReceiptRequest`.
   - Response DTOs: `FeeCategoryResponse`, `FeeStructureResponse`, `StudentFeeResponse`, `FeeSummaryResponse`, `ReceiptResponse`, `DashboardStatsResponse`.

3. **Repositories (`com.school.fee.repository`)**:
   - Spring Data JPA repositories with custom JPQL queries for total outstanding amounts, total paid amounts, students with due fees, and recent receipts.

4. **Service Layer (`com.school.fee.service`)**:
   - `AdminFeeService`: Logic for categories, structures, and fee assignments.
   - `ReceiptService`: Logic to calculate `paidAmount`/`dueAmount`, update `FeeStatus` (PENDING, PARTIAL, PAID), and issue receipts.
   - `ParentFeeService`: Logic for fee summaries and student payment status.
   - `FeeDashboardService`: Logic for aggregate metrics and dashboard stats.

5. **Global Exception Handling (`com.school.fee.exception`)**:
   - `@RestControllerAdvice` handling `ResourceNotFoundException`, `BusinessRuleException`, `DuplicateResourceException`, and `MethodArgumentNotValidException`.
   - Standardized JSON `ErrorResponse` record containing `status`, `error`, `message`, `validationErrors` map, and `timestamp`.

6. **Controllers & OpenAPI Specs**:
   - Implement all 3 controllers using Lombok `@RequiredArgsConstructor` constructor injection.
   - Annotate endpoints with Swagger/OpenAPI `@Tag` and `@Operation`.

7. **Security Setup (`com.school.fee.config`)**:
   - `SecurityConfig` with stateless JWT-ready authorization rules (`/api/v1/admin/**`, `/api/v1/teacher/**`, `/api/v1/parent/**`) and `@EnableMethodSecurity`.

Generate full, compilable code for each file with no truncated placeholders, mock comments, or omitted code.
