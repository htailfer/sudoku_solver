
from __future__ import print_function
from ortools.constraint_solver import pywrapcp

def main():

  # For profiling
  #solver_parameters=pywrapcp.SolverParameters()
  #solver_parameters.profile_level = pywrapcp.SolverParameters.NORMAL_PROFILING


  # Create the solver.
  #solver = pywrapcp.Solver('sudoku', solver_parameters)
  solver = pywrapcp.Solver('sudoku')
  block_size = 3
  line_size = block_size ** 2
  line = range(0, line_size)
  block = range(0, block_size)

  initial_grid = [[0, 4,  0, 1, 0, 0, 0, 0, 0],
                  [0, 0,  3, 5, 0, 0, 0, 1, 9],
                  
                  [0, 0,  0, 0, 0, 6, 0, 0, 3],
                  [0, 0,  7, 0, 0, 5, 0, 0, 8],
                  [0, 8,  1, 0, 0, 0, 9, 6, 0],
                  [9, 0,  0, 2, 0, 0, 7, 0, 0],
                  [6, 0,  0, 9, 0, 0, 0, 0, 0],
                  [8, 1,  0, 0, 0, 2, 4, 0, 0],
                  [0, 0,  0, 0, 0, 4, 0, 9, 0]]

  grid = {}
  for i in line:
    for j in line:
      grid[(i, j)] = solver.IntVar(1, line_size, 'grid %i %i' % (i, j))

  # AllDifferent on rows.
  for i in line:
    solver.Add(solver.AllDifferent([grid[(i, j)] for j in line]))

  # AllDifferent on columns.
  for j in line:
    solver.Add(solver.AllDifferent([grid[(i, j)] for i in line]))

  # AllDifferent on blocks.
  for i in block:
    for j in block:
      one_block = []
      for di in block:
        for dj in block:
          one_block.append(grid[(i * block_size + di, j * block_size + dj)])

      solver.Add(solver.AllDifferent(one_block))

  # Initial values.
  for i in line:
    for j in line:
      if initial_grid[i][j]:
        solver.Add(grid[(i, j)] == initial_grid[i][j])

  all_vars = [grid[(i, j)] for i in line for j in line]

  db = solver.Phase(all_vars,
                    solver.INT_VAR_SIMPLE,
                    solver.INT_VALUE_SIMPLE)

  # And solve.
  solver.NewSearch(db)

  while solver.NextSolution():
    for i in line:
        print ([int(grid[(i, j)].Value())for j in line])
  
  print ("Nombre de solutions:", solver.Solutions())
  print ("Echecs:", solver.Failures())
  print ("Branches:", solver.Branches())
  print ("wall_time:", solver.WallTime())


if __name__ == '__main__':
  main()
