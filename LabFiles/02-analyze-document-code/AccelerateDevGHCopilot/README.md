# Library App

## Description

Library App is a .NET 9 console application that helps library staff manage patrons, memberships, and book loans. It serves as a temporary library management solution using JSON files for data storage.

Users can:

- Search for patrons by name using a case-sensitive search.
- View patron membership expiration dates.
- View a patron’s book loans.
- Mark borrowed books as returned.
- Request loan extensions and membership renewals, subject to application rules.

The application separates the console interface, business logic, and data access into individual projects. It also includes an xUnit test project for the application services.

## Project Structure

- `src/`
  - `Library.ApplicationCore/`
    - `Entities/` — Library data models.
    - `Enums/` — Status values used by application operations.
    - `Interfaces/` — Service and repository contracts.
    - `Services/` — Loan and patron business rules.
  - `Library.Console/`
    - `Json/` — Sample records for authors, books, book items, patrons, and loans.
    - `Program.cs` — Configuration, dependency injection, and application startup.
    - `ConsoleApp.cs` — Interactive console workflow.
    - `ConsoleState.cs` — Console application states.
    - `CommonActions.cs` — Shared console actions.
    - `appSettings.json` — Configuration for JSON file paths.
  - `Library.Infrastructure/`
    - `Data/`
      - `JsonData.cs` — Loads, saves, and connects JSON records.
      - `JsonLoanRepository.cs` — Retrieves and updates loan records.
      - `JsonPatronRepository.cs` — Searches, retrieves, and updates patron records.
- `tests/`
  - `UnitTests/`
    - `ApplicationCore/`
      - `LoanService/` — Tests for loan operations.
      - `PatronService/` — Tests for patron operations.
    - `LoanFactory.cs` — Creates loan data for tests.
    - `PatronFactory.cs` — Creates patron data for tests.

## Key Classes and Interfaces

### Console Interface

- **Program** configures the application, registers dependencies, and starts the console workflow.
- **ConsoleApp** handles user input and displays patron, membership, and loan information.

### Application Services

- **LoanService** implements business rules for returning books and extending loans.
- **PatronService** implements business rules for renewing memberships.
- **ILoanService** defines the loan service contract.
- **IPatronService** defines the patron service contract.

### Data Access

- **ILoanRepository** defines operations for retrieving and updating loans.
- **IPatronRepository** defines operations for searching, retrieving, and updating patrons.
- **JsonLoanRepository** implements loan data access using JSON-backed records.
- **JsonPatronRepository** implements patron data access using JSON-backed records.
- **JsonData** manages loading and saving JSON files and populates relationships between entities.

### Data Relationships

Records are stored separately and connected through IDs. For example, a loan identifies a patron and a book item. The book item connects to a book, which connects to an author.

`JsonData` populates these relationships in memory so the application can display related information. When saving patrons or loans, it writes IDs and scalar properties instead of nested related objects.

## Usage

### Prerequisites

- The .NET 9 SDK.
- Git, if cloning the repository.

Visual Studio Code with C# Dev Kit is recommended for exploring the project. GitHub Copilot is used for the training exercise but is not required to run the application.

### Open the Exercise Folder

If you have not cloned the repository:

```bash
git clone https://github.com/tabner0320/mslearn-github-copilot-dev.git
```

From the cloned repository’s root folder:

```bash
cd LabFiles/02-analyze-document-code/AccelerateDevGHCopilot
```

The following build and test commands assume this solution folder is your current directory.

### Build

```bash
dotnet build src/Library.Console/Library.Console.csproj
```

### Run

Change to the console project folder before running:

```bash
cd src/Library.Console
dotnet run
```

The application resolves `appSettings.json` and its configured JSON paths relative to the current working directory.

### Example Workflow

1. Enter `One` when prompted to search for a patron.
2. Enter `2` to select Patron One from the sample results.
3. Enter `1` to select the first loan.
4. Enter `r` to mark the book as returned.
5. Confirm that the application displays `Book was successfully returned.` and `Returned: True`.
6. Enter `s` to search again and verify the updated loan status.
7. Enter `q` to quit.

Available commands depend on the current screen:

- `s` — Start a new patron search.
- `q` — Quit the application.
- `m` — Request a membership renewal.
- `r` — Mark the selected loan as returned.
- `e` — Request an extension for the selected loan.
- A listed number — Select a patron or loan.

The sample records contain historical dates. Membership renewals and loan extensions are subject to the application’s business rules.

Returning a book or updating a membership changes the corresponding JSON data.

### Run Tests

If you are still in `src/Library.Console`, return to the solution folder:

```bash
cd ../..
```

Run the xUnit tests:

```bash
dotnet test tests/UnitTests/UnitTests.csproj
```

## License

This project is provided under the MIT License. See the
[repository license](../../../LICENSE) for details.