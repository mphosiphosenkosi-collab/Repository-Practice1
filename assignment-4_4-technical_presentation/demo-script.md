# Demo Script: Conference Room Booking System

## Demo Overview
**Duration:** 3-4 minutes
**Purpose:** Demonstrate key workflows and technical implementation
**Audience:** Technical team members and stakeholders

---

## Part 1: Repository Structure (60 seconds)

### What I'm Showing:

Conference-Room-Booking-System/
├── .github/ # GitHub workflows & templates
├── docs/api/ # API documentation
├── sprints/ # Agile tracking
├── src/ # Source code
└── key configuration files


### What I'm Saying:

"This is our project structure. Notice the clean separation: `.github` contains our CI/CD workflows and PR templates. `docs/api` holds our OpenAPI specifications. `sprints` contains our Agile artifacts. And `src` is where our implementation code lives. This organization makes it easy for new developers to navigate the project."

### Key Commands to Run:

tree -L 2  # Show directory structure
cat README.md | head -20  # Show project overview

# Part 2: API Contract Validation (90 seconds)

## What I'm Showing:

Open Postman

Import docs/api/api-documentation.yaml

Show generated collection structure

Run tests on GET /rooms endpoint


### What I'm Saying:

"Here's our API contract in action. We import the OpenAPI spec into Postman, which automatically creates a collection with all endpoints. Notice the structured folders for Rooms, Bookings, and Availability. When I run the tests, you can see automated validation of response formats, status codes, and data structures. This ensures our API behaves exactly as specified."

### Key Actions:

Import API spec into Postman

Show collection with 9 endpoints

Run test suite showing all green checks

Demonstrate mock server responses

## Part 3: Development Workflow (60 seconds)

### What I'm Showing:

bash
### Create a feature branch
git checkout -b feature/add-room-filter

### Make a change
echo "// New filter functionality" >> src/backend/filters.js

### Commit with conventional message
git add .
git commit -m "feat(filters): add capacity range filtering"

#### Show PR creation
git push origin feature/add-room-filter

## What I'm Saying:

"Now let's walk through our development workflow. We create a feature branch, implement changes with clear commit messages, and push to GitHub. When we create a pull request, you'll see our template automatically populates with checklists and guidelines. This standardization ensures quality and consistency across all contributions."

### Key Points to Highlight:

Branch naming convention

Conventional commit messages

PR template auto-fill

Automated checks running

## Part 4: CI/CD Pipeline (30 seconds)

### What I'm Showing:

GitHub Actions tab showing:

Recent workflow runs

Validation steps passing

Test execution results

Documentation checks

### What I'm Saying:

"Finally, our CI/CD pipeline automatically validates every change. When I created that commit, GitHub Actions immediately ran: checking our project structure, validating documentation, testing API contracts, and ensuring code quality. All green checks mean the change is ready for review. This automation catches issues early and maintains our quality standards."

### Key Metrics to Point Out:

Build time under 1 minute

100% test pass rate

Documentation validation

No security warnings

## Demo Conclusion (30 seconds)

### Summary Statement:

"This demo shows our end-to-end workflow: from well-organized code structure to formal API contracts, through standardized development processes, backed by automated quality assurance. Every piece is documented, testable, and follows professional standards."

### Call to Action:

"For new developers: clone the repo, run npm install && npm start, and explore the documentation. The system is designed to be approachable while maintaining enterprise-grade quality standards."

### Troubleshooting Notes:

If Postman import fails: Use local mock server on port 3000

If git commands fail: Ensure Git is installed and configured

If tests fail: Check that json-server is running on port 3000

Always have backup screenshots ready