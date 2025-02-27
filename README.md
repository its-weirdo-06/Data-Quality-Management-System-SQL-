# Data-Quality-Management-System-SQL-
Build a Data Quality Management System to ensure the accuracy, consistency, and completeness of data. The system will include data validation rules, data cleansing processes, and quality metrics.
### **Key Features:**

- Data validation rules
- Data cleansing processes
- Quality metrics and reporting
- Alerts for data quality issues

### **Key Tables:**

- `data_sources`
- `validation_rules`
- `data_issues`
- `quality_metrics`

### Script :

-- Create table for data sources

CREATE TABLE data_sources (
source_id INT IDENTITY(1,1) PRIMARY KEY, -- Use INT and IDENTITY for auto-increment
source_name VARCHAR(50) NOT NULL,
description TEXT
);

-- Create table for validation rules

CREATE TABLE validation_rules (
rule_id INT IDENTITY(1,1) PRIMARY KEY, -- Use INT and IDENTITY for auto-increment
source_id INT NOT NULL,
rule_description TEXT NOT NULL,
created_at DATETIME DEFAULT GETDATE(), -- Use DATETIME for timestamp, and GETDATE() for default
FOREIGN KEY (source_id) REFERENCES data_sources(source_id)
);

-- Create table for data issues

CREATE TABLE data_issues (
issue_id INT IDENTITY(1,1) PRIMARY KEY, -- Use INT and IDENTITY for auto-increment
source_id INT NOT NULL,
issue_description TEXT NOT NULL,
detected_at DATETIME DEFAULT GETDATE(), -- Use DATETIME for timestamp, and GETDATE() for default
status VARCHAR(20) NOT NULL,
resolved_at DATETIME, -- Use DATETIME for timestamp
FOREIGN KEY (source_id) REFERENCES data_sources(source_id)
);

-- Create table for quality metrics

CREATE TABLE quality_metrics (
metric_id INT IDENTITY(1,1) PRIMARY KEY, -- Use INT and IDENTITY for auto-increment
source_id INT NOT NULL,
metric_name VARCHAR(50) NOT NULL,
metric_value DECIMAL(18, 2), -- Use DECIMAL for numeric values
recorded_at DATETIME DEFAULT GETDATE(), -- Use DATETIME for timestamp
FOREIGN KEY (source_id) REFERENCES data_sources(source_id)
);

-- Sample query to get the number of data issues by source

SELECT source_name, COUNT(*) AS issue_count
FROM data_issues
JOIN data_sources ON data_issues.source_id = data_sources.source_id
GROUP BY source_name;
