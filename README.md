Make a map of Department to List of employee from employee object containing list of departments

Question: Employee contains list of departments and we have a list of employees now how to obtain a map like Map<Department,List of Employees> from employee list.

The below code does the trick but I wanted to know how to use streams api effectively instead of for loop.

   Department a = new Department("a");
    Department b = new Department("b");
    Department c = new Department("c");

    Employee e1 = new Employee("e1", List.of(a, b));
    Employee e2 = new Employee("e2", List.of(c, b));
    Employee e3 = new Employee("e3", List.of(c, a));
    Employee e4 = new Employee("e4", List.of(a, b, c));

    List<Employee> employees = List.of(e1, e2, e3, e4);
    Set<Department> departments = employees.stream().flatMap(employee ->
            employee.getDepartments().stream()).collect(Collectors.toSet());

    for (Department d : departments) {
        for (Employee employee : employees) {
            if (employee.getDepartments().contains(d)) {
                if (!result.containsKey(d)) {
                    result.put(d, new ArrayList<Employee>());
                }
                result.get(d).add(employee);
            }
        }
    }
    return result;


    =============================
    [...]
List<Employee> employees = List.of(e1, e2, e3, e4);
for (Employee employee : employees) {
    for (Department d : employee.getDepartments()) {
        if (!result.containsKey(d)) {
            result.put(d, new ArrayList<Employee>());
        }
        result.get(d).add(employee);
    }
}
return result;


--
[...]
List<Employee> employees = List.of(e1, e2, e3, e4);
var result = employees.stream().flatMap(employee->employee.getDepartments().stream()
        .map(department->AbstractMap.SimpleImmutableEntry<>(department,employee))
    .collect(Collectors.groupingBy(pair->pair.getKey()));

    
