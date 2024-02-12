# Job Framework

## Steps to creating a job

- Define the steps: Parallel or Sequential
- Use framework's predefined steps as much as possible

### Creating a Job Step

- Choose from the list of predefined steps
- Configure the step
- Finalize the job

### Creating a Custom Job Step

- Define a `steptypes.json` file in our cartridge
- Create a script that implements the function defined in `steptypes.json`
- Find the custom step in BM and configure it
- To add a custom status code "No_Files_Found":
```
const Status = require('dw/system/Status);

return new Status(Status.OK, 'No_Files_Found');
```