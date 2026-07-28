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
  






========================
/opspec propose

Generate Oracle SQL scripts (`data.sql`) to populate the existing School Management System database with realistic test data for development, API testing, dashboard validation, and reporting.

## Instructions

- Analyze all existing JPA `@Entity` classes in the project.
- Use the existing table names, column names, foreign keys, constraints, enums, and relationships.
- Do NOT create any DDL (CREATE TABLE, ALTER TABLE, DROP TABLE).
- Generate only Oracle-compatible INSERT or MERGE statements.
- Maintain the correct insertion order to satisfy all foreign-key constraints.
- End the script with COMMIT.

## Test Data Requirements

Generate realistic demo data for the following:

### Schools
- Create 5 schools.
- Each school should have a unique details

### Admin Users
- Create 1 Admin user for each school.
- Total: 5 Admin users.
- Assign each Admin to the corresponding school.

### Teachers
- Create 5 teachers for each school.
- Total: 25 teachers.
- Generate realistic names, employee IDs, email addresses, mobile numbers, gender, qualification, joining date, and ACTIVE status.
- Assign teachers to schools, classes, sections, and subjects based on the existing schema.

### Parents
- Create 20 parents for each school.
- Total: 100 parents.
- Generate realistic father name, mother name, email, mobile number, occupation, and address.

### Students
- Create 20 students for each school.
- Total: 100 students.
- Generate realistic admission numbers, roll numbers, names, gender, DOB, admission date, blood group, and ACTIVE status.
- Associate every student with:
  - School
  - Parent
  - Class
  - Section
- Each parent should have between 1 and 2 students.

### Academic Data
If the project already contains Academic Year, Class, Section, Subject, or Teacher Assignment tables, populate them with realistic data and establish proper relationships.

### Attendance
If the Attendance module exists:
- Generate attendance data for the last 30 days.
- Include Present, Absent, Late, Half Day, and Leave records.
- Ensure some students have attendance below 75% for dashboard testing.

### Fee Module
If the Fee module exists:
- Generate Fee Categories.
- Generate Fee Structures.
- Assign fees to students.
- Create paid, partially paid, pending, and overdue fee records.
- Generate manual receipt records.

## Dashboard Support

Generate enough data so the following dashboards display meaningful information:

- Total Schools
- Total Admins
- Total Teachers
- Total Parents
- Total Students
- Students Present Today
- Students Absent Today
- Attendance Percentage
- Students Below 75% Attendance
- Outstanding Fees
- Paid Fees
- Recent Receipts
- Monthly Admissions

## SQL Requirements

- Generate Oracle-compatible SQL only.
- Use INSERT or MERGE statements as appropriate.
- Respect all foreign-key relationships.
- Insert parent records before child records.
- Include comments for each section.
- End the script with COMMIT.

## Expected Output

Generate a single executable Oracle `data.sql` file that can be run directly using SQL Developer or SQLcl.

The generated data should be sufficient to test:
- Authentication
- CRUD operations
- Search and Filters
- Pagination
- Attendance Module
- Fee Module
- Dashboards
- Reports
- Analytics

The SQL should execute successfully on a fresh database without requiring manual modifications.

7. **Security Setup (`com.school.fee.config`)**:
   - `SecurityConfig` with stateless JWT-ready authorization rules (`/api/v1/admin/**`, `/api/v1/teacher/**`, `/api/v1/parent/**`) and `@EnableMethodSecurity`.

Generate full, compilable code for each file with no truncated placeholders, mock comments, or omitted code.
