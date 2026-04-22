# Employee Dashboard - Debugging Assignment

A Next.js employee management application with some bugs that need fixing.

## Your Mission

This employee dashboard application is **almost working**, but several features are broken. Your task is to find and fix all the bugs to make the application fully functional.

## Getting Started

### Installation

1. Install dependencies:
```
npm install
```

2. Create a `.env.local` file (if not present) with:
```
API_URL=your_api_url_here
```

3. Start the development server:
```
npm run dev
```

## Expected Features

The application should support:

- **View Employees** - Display a list of all employees
- **Add Employee** - Create new employee records
- **Edit Employee** - Update existing employee information
- **Delete Employee** - Remove employees from the system
- **Sort Employees** - Sort by name, hire date, or job title
- **View Details** - See individual employee details
- **Edit Details** - Modify employee details from the detail page

## Known Issues

The application currently has several bugs affecting functionality. Your job is to:

1. **Identify** all bugs
2. **Document** what's broken and why
3. **Fix** each bug
4. **Verify** the fix works correctly

## Testing Your Fixes

After fixing bugs, verify these features work:

### Employee List
- [x] Employees load on page mount
- [x] Sorting by name (A-Z and Z-A) works
- [x] Sorting by hire date (newest/oldest) works
- [x] Filtering by job title works
- [x] List updates immediately after changes

### Add Employee
- [ ] Modal opens with empty form
- [ ] Can enter name, job title, and hire date
- [ ] New employee appears in list immediately
- [ ] Modal closes after successful add

### Edit Employee
- [ ] Modal opens with pre-filled data
- [ ] Can modify employee information
- [ ] Changes appear in list immediately
- [ ] Modal closes after successful edit

### Delete Employee
- [ ] Employee is removed from list immediately
- [ ] No page refresh required

### Employee Details
- [ ] Can view individual employee details
- [ ] Edit button switches to edit mode
- [ ] Edit mode shows input fields
- [ ] Can save changes from detail page
- [ ] Back button returns to employee list

## Submission

When complete, document:

1. **List of bugs found** - What was broken?
2. **Root causes** - Why did each bug occur?
3. **Fixes applied** - How did you fix each bug?
4. **Testing results** - How did you verify the fixes?


1. 
in employee list : 
    const clearEmployeeCache = async () => {
        try {
            // const result: Employee[] | "Not Authorized" = [];
            // const result = await getEmployeesAction();
             const result = { success: false, data: [] };

>>>


            const clearEmployeeCache = async () => {
        try {
            // const result: Employee[] | "Not Authorized" = [];
            const result = await getEmployeesAction();
            // const result = { success: false, data: [] };

1. was not displaying employees
2. the function that got the employees was commented out
3. switching the result commented out
4. checking to see if the data was displayed

2. 
in employee list :

    const handleViewEmployee = async (id: number) => {
        push(`/employees/${id}`);
    };

    // Fetching employees after token is set
    useEffect(() => {
        const handleGet = async () => {
            await clearEmployeeCache();
        }

        handleGet();
    }, [clearEmployeeCache])

>>>
    
    ...     
    }, [])

1. infinite loop loading data
2. the clearEmployeeCache in the brackets meant that every time it changed it would re-render, but it would change every time it ran creating an infinite loop
3. taking the clearEmployeeCache out of the brackets so it would render once on load
4. checking in the console to see if I was still getting infinite messages

3. 
in employee list, sorting section

case "name":
                    setEmployees(sortingEmployees.sort((a: Employee, b: Employee) => a.name.localeCompare(b.name)));
                    break;
                case "name-reverse":
                    setEmployees(sortingEmployees.sort((a: Employee, b: Employee) => b.name.localeCompare(a.name)));
                    break;
>>>
case "name":
                    setEmployees(sortingEmployees.sort((a: Employee, b: Employee) => b.name.localeCompare(a.name)));
                    break;
                case "name-reverse":
                    setEmployees(sortingEmployees.sort((a: Employee, b: Employee) => a.name.localeCompare(b.name)));
                    break;

1. the a-z and z-a were reversed
2. the order of the b and a were swapped in the cases
3. changed a.name.localeCompare(b.name) to b.name.localeCompare(a.name), and vice versa
4. clicked a-z and z-a to see if they sorted properly

4. 
in Employee list, sorting section

                case "hire-date":
                    setEmployees(sortingEmployees.sort(
                        (a: Employee, b: Employee) => Number(new Date(b.hireDate)) - Number(new Date(a.hireDate))

                    ));
                    break;
                case "hire-date-reverse":
                    setEmployees(sortingEmployees.sort(
                        (a: Employee, b: Employee) => Number(new Date(a.hireDate)) - Number(new Date(b.hireDate))
                    ));
>>>
                case "hire-date":
                    setEmployees(sortingEmployees.sort(
                    (a: Employee, b: Employee) => Number(new Date(a.hireDate)) - Number(new Date(b.hireDate))

                    ));
                    break;
                case "hire-date-reverse":
                    setEmployees(sortingEmployees.sort(
                    (a: Employee, b: Employee) => Number(new Date(b.hireDate)) - Number(new Date(a.hireDate))

                    ));

                    
1. the hire date oldest and newest were reversed
2. the logic was swapped in the two cases
3. cut and pasted the logic in each sort into the opposite ordered case.
4. clicked newest and oldest to see if they sorted properly

5. 
{deletedEmployees.length === 0 ? (
                        <TableRow>
                            <TableCell></TableCell>
                            <TableCell className="text-center">
                                No Employees
                            </TableCell>
                            <TableCell></TableCell>
                        </TableRow>
                    ) : (
                        employees.map((employee, idx) => (
>>>
{deletedEmployees.length === 0 ? (
                        <TableRow>
                            <TableCell></TableCell>
                            <TableCell className="text-center">
                                No Employees
                            </TableCell>
                            <TableCell></TableCell>
                        </TableRow>
                    ) : (
                        employees.map((employee, idx) => (

1. job filter wasn't working
2. the sorting for job filters was setting Employees but deletedEmployees was being mapped, so nothing would update/display
3. changed deletedEmployees rendering to Employees rendering
4. clicked different job filters to see if they sorted properly


6.    in Employee form
const resetForm = () => {
        if (type !== "Add")
             {
            setOriginalEmployee(employee as Employee);
        }

>>>

const resetForm = () => {
        if (type !== "Add")
             {
            setOriginalEmployee(employee as Employee);
        }

1. employee.name was coming up as null in add
2. we want OriginalEmployee to be set as employee when editing, not adding. if it's set when there's no value because it's not being edited, it'll come up as null
3. added a not operator
4. clicked on add to see if the form came up

in Employee Form

            if (type === "Add") {
                const result = await addEmployeeAction(employeeWithChanges);
                if (result.success) {
                    refreshEmployees();
                }
            } else {
                const result = await updateEmployeeAction(employeeWithChanges);
                if (result.success) {
                    refreshEmployees();

                }
            }

            setOriginalEmployee({
                id: 0,
                name: "",
                jobTitle: "",
                hireDate: "",
                details: "",
                status: ""
            });
        } catch (error) {
            console.log("error", error);
        }

        submitForm();

    };

>>>
            if (type === "Add") {
                const result = await addEmployeeAction(employeeWithChanges);
                if (result.success) {
                    // refreshEmployees();
                    console.log("add was successful")
                }
            } else {
                const result = await updateEmployeeAction(employeeWithChanges);
                if (result.success) {
                    // refreshEmployees();
                    console.log("edit was successful")

                }
            }

            setOriginalEmployee({
                id: 0,
                name: "",
                jobTitle: "",
                hireDate: "",
                details: "",
                status: ""
            });
        } catch (error) {
            console.log("error", error);
        }

        submitForm();
        refreshEmployees();

    };

1. it didn't seem like the data was refreshing
2. order of operations, we were refreshing employees before submitting the form
3. moved refreshEmployees to after submit form
4. submitted a new employee to see if it automatically refreshed with the new one listed