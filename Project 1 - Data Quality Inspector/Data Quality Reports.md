# Data Quality Reports

## Overview

Terminal output is useful during development, but saved reports make inspection results easier to review and share.

The project generates Markdown and HTML reports from the analysis results.

## Markdown report

Markdown is a simple text format that works well in repositories and documentation.

A report can include:

- dataset summary;
- warnings;
- tables;
- recommendations.

## HTML report

HTML provides a browser-friendly version of the same findings. It is useful when someone wants to review the results without running the command themselves.

## Design principle

The analysis should produce structured results once. Markdown and HTML should render those same results instead of duplicating inspection logic.

This keeps the reports consistent and easier to maintain.

## Logging and configuration

This project uses configuration-driven logging.

Logging records important events, such as an inspection starting, completing, or failing. It helps diagnose command-line application behaviour without mixing technical diagnostic messages into the report itself.

## Key takeaway

Reports make an inspection script easier to review and share. Logging makes the application easier to understand when something goes wrong.

## Related notes

- [Data Quality Checks and Validation](Data%20Quality%20Checks%20and%20Validation.md)
- [Data Quality Warnings for ML](Data%20Quality%20Warnings%20for%20ML.md)