# ai

def dpll(clauses, assignment):
    if not clauses:
        return True, assignment
    
    for clause in clauses:
        if not clause:
            return False, None

    unit_clauses = [c[0] for c in clauses if len(c) == 1]
    if unit_clauses:
        for unit in unit_clauses:
            new_assignment = assignment.copy()
            new_assignment[abs(unit)] = unit > 0
            new_clauses = propagate_unit_clause(clauses, unit)
            return dpll(new_clauses, new_assignment)

    literal = clauses[0][0]

    new_assignment = assignment.copy()
    new_assignment[abs(literal)] = True
    new_clauses = propagate_literal(clauses, literal)
    satisfiable, final_assignment = dpll(new_clauses, new_assignment)
    if satisfiable:
        return True, final_assignment

    new_assignment = assignment.copy()
    new_assignment[abs(literal)] = False
    new_clauses = propagate_literal(clauses, -literal)
    return dpll(new_clauses, new_assignment)

def propagate_unit_clause(clauses, unit):
    new_clauses = []
    for clause in clauses:
        if unit in clause:
            continue  
        new_clause = [l for l in clause if l != -unit]
        new_clauses.append(new_clause)
    return new_clauses

def propagate_literal(clauses, literal):
    new_clauses = []
    for clause in clauses:
        if literal in clause:
            continue  
        new_clause = [l for l in clause if l != -literal]
        new_clauses.append(new_clause)
    return new_clauses

def main():
    num_clauses = int(input("Enter the number of clauses: "))
    clauses = []

    for i in range(num_clauses):
        clause_input = input(f"Enter clause {i + 1} (space-separated literals, use negative for NOT): ")
        clause = list(map(int, clause_input.split()))
        clauses.append(clause)

    satisfiable, assignment = dpll(clauses, {})
    if satisfiable:
        print("Satisfiable with assignment:", assignment)
    else:
        print("Unsatisfiable")

if __name__ == "__main__":
    main()