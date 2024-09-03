# multigrid


Multigrid Methods are a class of algorithms used to solve large linear systems of equations, particularly those arising from the discretization of partial differential equations (PDEs). These methods are highly efficient because they accelerate the convergence of iterative solvers by operating on multiple levels of grid resolution, from fine to coarse grids.


Consider solving the linear system Ax = b, where A is a large sparse matrix that arises from discretizing a PDE on a grid. The goal is to find the solution vector x.

Multigrid methods operate on a hierarchy of grids, typically consisting of a fine grid (high resolution) and several coarser grids (lower resolutions). The idea is to solve the problem on multiple grids, using coarse grids to eliminate low-frequency errors (smooth components) and fine grids to eliminate high-frequency errors (oscillatory components).

Smoothing:
Smoothing refers to the process of reducing high-frequency errors in the solution. This is typically done using a relaxation method, such as Gauss-Seidel or Jacobi iteration.
High-frequency errors are reduced effectively on fine grids but not on coarse grids.

Restriction:
Restriction is the process of transferring the residual (the error in the current solution) from a fine grid to a coarser grid. The residual on the coarse grid helps to correct the low-frequency errors that are not efficiently reduced on the fine grid.

Prolongation (Interpolation):
Prolongation is the process of transferring the correction from a coarse grid back to a fine grid.
This correction is then applied to the fine grid solution to improve its accuracy.

V-Cycle:
The V-Cycle is a typical sequence of operations in a multigrid method:
Pre-smoothing: Apply a few iterations of a relaxation method on the fine grid.

Restriction: Transfer the residual to the next coarser grid.
Coarse Grid Correction: Solve the residual equation on the coarse grid (either exactly or approximately using another multigrid method).

Prolongation: Transfer the correction back to the fine grid.

Post-smoothing: Apply a few more iterations of the relaxation method on the fine grid.

Convergence:
Multigrid methods are highly efficient because they combine the strengths of both fine and coarse grids. The fine grids effectively reduce high-frequency errors, while the coarse grids handle low-frequency errors.
The result is a method that can achieve very rapid convergence, often in a number of iterations that is proportional to the number of unknowns


Pseudocode
function V_Cycle(u, f, level, max_level):
    if level == max_level:
        u = Relax(u, f)
    else:
        u = Relax(u, f)
        residual = f - A*u
        residual_coarse = Restrict(residual)
        e_coarse = V_Cycle(zeros_like(residual_coarse), residual_coarse, level+1, max_level)
        e_fine = Prolong(e_coarse)
        u = u + e_fine
        u = Relax(u, f)
    return u


Multigrid Method
Discretize the domain into a grid.
Initialize the solution u and compute the initial residual r = f - Au.
V-Cycle:
Perform pre-smoothing using a relaxation method on the fine grid.
Restrict the residual to a coarser grid.
Solve the residual equation on the coarse grid using another V-Cycle.
Prolong the correction back to the fine grid.
Apply post-smoothing on the fine grid.
Iterate until the residual is sufficiently small.

