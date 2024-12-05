# Hands-on-Message-brokers

## Table of contents

* [Team Information](#team-information)
* [Demo](#demo)
* [Report](#report)
* [Project Structure](#project-structure)
* [How To](#how-to)

## Team Information

### Team 11

| Full name       | Group     | Email                           |
|-----------------|-----------|---------------------------------|
| Azamat Bayramov | B22-SD-03 | a.bayramov@innopolis.university |
| Darya Koncheva  | B22-SD-02 | d.koncheva@innopolis.university |
| Matthew Rusakov | B22-SD-03 | m.rusakov@innopolis.university  |
| Egor Valikov    | B22-CBS-01| e.valikov@innopolis.university  |

## Demo

Demo is available on YouTube: [Link](https://youtu.be/6ck_1lZ0SNU)

## Report

#### 1. Resourse analysis

On the first screenshot you may see for single case:
![Single - resourses](https://github.com/iu-f24-sa-t11/Hands-on-Message-brokers/raw/main/static/single-1.png)
and on the second screenshot - for multiple case:
![Multiple - resourses](https://github.com/iu-f24-sa-t11/Hands-on-Message-brokers/raw/main/static/multiple-1.jpg)

 As you can see on these screenshots:
 - In the **single setup**, a **single container** is consuming a **high amount of CPU resources**, leading to a potential bottleneck.
 - In contrast, the **multiple setup** distributes the load **evenly across containers**, resulting in better utilization and balanced resource consumption.

#### 2. Response times analysis

On the first screenshot you may see for single case:
![Single - resourses](https://github.com/iu-f24-sa-t11/Hands-on-Message-brokers/raw/main/static/single-2.jpg)
and on the second screenshot - for multiple case:
![Multiple - resourses](https://github.com/iu-f24-sa-t11/Hands-on-Message-brokers/raw/main/static/multiple-2.jpg)

For better comparing we plot graphics:
Single:
![Single - resourses](https://github.com/iu-f24-sa-t11/Hands-on-Message-brokers/raw/main/static/single-3.jpg)
Multiple:
![Multiple - resourses](https://github.com/iu-f24-sa-t11/Hands-on-Message-brokers/raw/main/static/multiple-3.jpg)

We can see that:
  - **Average**, **maximum**, and **percentile (50th and 95th)** response times are **lower** compared to the **multiple setup**.
  - This is because the **single setup** does not have a connection to **RabbitMQ**, which reduces the overhead in processing.


## Project Structure

```
Hands-on-Message-brokers/
├── src/
│   ├── processors/
│   │   ├── __init__.py          # Base class for processors managing message queues and processing logic
│   │   ├── api.py               # Processor exposing an API to receive and dispatch messagesc
│   │   ├── filter.py            # Processor filtering messages based on stop words
│   │   ├── screaming.py         # Processor converting message text to uppercase
│   │   ├── sending.py           # Processor sending messages via email using SMTP
│   ├── queues/
│   │   ├── __init__.py          # Abstract base class for queues handling message retrieval and delivery
│   │   ├── multiprocess.py      # Queue implementation using Python's multiprocessing queue
│   │   ├── rabbitmq.py          # Queue implementation using RabbitMQ for message handling
│   ├── schemas/
│   │   ├── __init__.py          # Initializes the schemas module and imports message schema
│   │   ├── message.py           # Defines the Message schema for user and text data
│   ├── services/
│   │   ├── __init__.py          # Initializes the services module
│   │   ├── api.py               # Service for receiving and forwarding messages via an API
│   │   ├── filter.py            # Service for filtering messages based on stop words
│   │   ├── screaming.py         # Service for converting message text to uppercase
│   │   ├── sending.py           # Service for sending messages via email
│   │   ├── single.py            # Standalone service chaining all processors using in-memory multiprocessing queues
│   ├── __init__.py              # Initializes the src module
│   ├── config.py                # Configuration file containing application settings and environment variables
├── test/
│   ├── __init__.py              # Initializes the test module
│   ├── locustfile.py            # Load testing script using Locust to simulate API requests
├── .env-template                # Template for environment variables required by the application
├── .gitignore                   # Specifies files and directories to ignore in version control
├── Dockerfile                   # Defines multi-stage builds for containerized services
├── README.md                    # Project documentation
├── docker-compose-multiple.yml  # Orchestrates multiple services with RabbitMQ in a containerized environment
├── docker-compose-single.yml    # Orchestrates the single-container version of the application
├── requirements-test.txt        # Dependencies required for testing, including Locust
├── requirements.txt             # Main application dependencies
├── run.sh                       # Bash script to switch between single and multiple container setups
```

## How To

### How to launch application

#### Single services (pipes and filters)

```
bash run.sh single
```

OR (just content of `run.sh` with `single` parameter)

```
docker compose -f docker-compose-multiple.yml down
docker compose -f docker-compose-single.yml up --build -d
```

#### Multiple services (broker)

```
bash run.sh multiple
```

OR (just content of `run.sh` with `multiple` parameter)

```
docker compose -f docker-compose-single.yml down
docker compose -f docker-compose-multiple.yml up --build -d
```

### How to use application

- Open http://localhost:8000/docs
- Send messages

### How to launch load test

- Install locally load testing tools (locust)
```
pip install -r requirements-test.txt
```

- Run locust
```
locust -f test/locustfile.py --host=http://localhost:8000
```

- Open web interface and start load test
