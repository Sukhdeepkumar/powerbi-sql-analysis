# powerbi-sql-analysis
SQL queries used for Power BI dashboard analysis
-- Creating a Common Table Expression (CTE) to combine upcoming and completed projects
WITH project_status AS (
    SELECT
        project_id,
        project_name,
        project_budget,
        'upcoming' AS status
    FROM project.`upcoming projects`

    UNION ALL

    SELECT
        project_id,
        project_name,
        project_budget,
        'completed' AS status
    FROM project.completed_projects
)

-- Main query to fetch employee details along with their assigned project info and department data
SELECT
    e.employee_id,
    e.first_name,
    e.last_name,
    e.job_title,
    e.salary,
    
    d.Department_Name,
    d.Department_Budget,
    d.Department_Goals,

    pa.project_id,
    ps.project_name,
    ps.project_budget,
    ps.status
FROM project.employees e

-- Join with departments to get department info for each employee
JOIN project.departments d
    ON e.department_id = d.Department_ID

-- Join with project_assignments to link employees to projects
JOIN project_assignments pa
    ON pa.employee_id = e.employee_id

-- Join with project_status (CTE) to get project details and status
JOIN project_status ps
    ON ps.project_id = pa.project_id;
