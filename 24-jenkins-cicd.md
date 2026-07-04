# 🔄 PHẦN 24: JENKINS CI/CD

> **Mục tiêu**: Integrate tests với Jenkins để tự động chạy.

## Jenkins là gì?

CI/CD tool để automate test execution.

## Setup

1. Install Jenkins
2. Create new job
3. Configure Git repository
4. Add build step: `mvn clean test`
5. Run job

## Jenkins Pipeline

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'your-repo-url'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn clean test'
            }
        }
    }
}
```

[← Bài trước](23-reporting.md) | [Bài tiếp →](25-best-practices-career.md)
