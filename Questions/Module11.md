### Question 6

What is AWS CloudFormation?

* [ ] A template that describes your infrastructure
* [x] An AWS service that you can use to create, model, and manage AWS resources
* [ ] A package of all the information that is needed to launch an Amazon EC2 instance
* [ ] A description of best practices for designing an AWS implementation

**Rationale:**
**AWS CloudFormation** is a managed service that allows users to **define, provision, and manage AWS infrastructure as code**. It uses templates (in JSON or YAML) to automate the deployment of AWS resources consistently and repeatedly.

---

### Question 7

What is AWS CloudFormation Designer?

* [ ] A source code repository for AWS CloudFormation templates
* [x] A graphical design interface for creating AWS CloudFormation templates
* [ ] A tool for automating deployments
* [ ] A collection of reusable templates

**Rationale:**
**AWS CloudFormation Designer** is a **visual tool** that helps users **create, view, and modify CloudFormation templates** using a drag-and-drop interface. It simplifies the design of complex infrastructures by allowing users to visually connect and configure AWS resources before generating or editing the underlying template code.

---

### Question 8

Which option can be used to accomplish deployment-specific differences in an AWS CloudFormation template?

* [ ] Use AWS CloudFormation Designer
* [ ] Use drift detection
* [ ] Use change sets
* [x] Use conditions

**Rationale:**
**Conditions** in AWS CloudFormation templates allow you to **control the creation of resources or configurations** based on specific parameters, environments, or deployment scenarios. For example, you can specify that certain resources should only be created in a production environment but not in development. This enables flexible, environment-specific deployments from a single template.

---

### Question 9

Which option is a good way to preview changes before implementing them in AWS CloudFormation Designer?

* [x] Create a change set
* [ ] Visually inspect the template
* [ ] Run Update Stack
* [ ] Run Detect Drift

**Rationale:**
A **change set** in AWS CloudFormation allows you to **preview how proposed modifications will affect your stack** before applying them. This feature helps identify potential resource replacements or deletions that might cause disruptions, ensuring safe and predictable updates to your infrastructure.

---

### Question 10

Which option is a good way to know which resources in an application environment were manually modified if the environment was created by running an AWS CloudFormation stack?

* [ ] Run a change set on the stack
* [ ] Run a comparison in AWS CloudFormation Designer on the stack
* [ ] Run conditions on the stack
* [x] Run drift detection on the stack

**Rationale:**
**Drift detection** in AWS CloudFormation identifies resources that have been **manually modified** outside of the stack management process. It compares the actual configuration of stack resources with the configuration defined in the stack template, flagging any discrepancies (“drift”) between the two.
