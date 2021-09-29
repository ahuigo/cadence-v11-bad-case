# Bad case about failed to set ScheduleToStartTimeout in cadence 0.11
## install cadence server 

    cd docker
    docker-compose up -d

## register domain

    #!/bin/bash
    front=172.21.65.94
    docker run --rm -it artifactory.momenta.works/docker-hdmap-dev/cadence:dev-20210127.1 cadence --ad $front:7933  --do hdmap-workflow domain register
## bad case
Try to set timout in `cmd/samples/recipes/greetings/greetings_workflow.go`:

    // SampleGreetingsWorkflow Workflow Decider.
    func SampleGreetingsWorkflow1(ctx workflow.Context) error {
        // Get Greeting.
        ao := workflow.ActivityOptions{
            ScheduleToStartTimeout: 12 * time.Minute,
            StartToCloseTimeout:    time.Minute,
            HeartbeatTimeout:       time.Second * 21,
        }


create a workflow and execute it:

    go run ./cmd/samples/recipes/greetings -m trigger
    go run ./cmd/samples/recipes/greetings -m worker

We could find `ScheduleToStartTimeout` do not work!
