## Infrastructure Cloud Run Week 8
## With cloud run, it is much more simple to set up than in weeks 3-5. There was no need for multiple machines, just strictly utilizing the Google Cloud shell.

## Deployment Command Week 8
## The deployment commands were also much more simple, but there would need to be configurtion with Google to ensure that the connectivity is there to make sure it runs smoothly. Github is also needed, and if there isn't authentication, it could cause a problem, GCP overall handles the deployment.

## TLS /HTTPS Week 8
## These were configured automatically by GCP.

## Scaling Approach Week 8
## It could be a bit cost effective if one would need to add more to the environment, since it is running off an organizaiton such as Google, there is a set of limitations until it could become costly.

## Port Management Week 8
## For port management it's main focus through week 8 wasn't all there too much, since GCP handles this.

## Cost when idle Week 8
## It depends on how much it's running, but if it's idle there is no costs.

## Rollback Week 8
## It could be a bit a bit trickier to re-deploy a image through google cloud. I noticed it might take a lot of manual work to ensure you're going back to what you want.

## Secrets Management Week 8
## For this week, it wasn't putting our 'secrets' in the actual secrets section where it would be harder to view, if lets say someone had access to your github and then would not be able to see what was the input. This week instead we used Variables, which does display the input.


## Reflection Questions

## Q1: Which approach required more manual steps from push to live URL? List the specific steps that were eliminated by Cloud Run.

## Definitely the Weeks 3-5 required more manual steps. Cloud Run removed the need to have seperate VMs to run this app.


## Q2: A security audit asks how you know which version of the code is currently running in production. How would you answer for on-premise Docker vs. Cloud Run with commit SHA tagging?

## With on premise, the images are tagged by the SHA and would verify this through the docker. Versus with Cloud run, you could deploy the revision image and show the SHA.

## Q3: Your on-premise VMs run 24/7 even when no students are using the app. Cloud Run scales to zero. What is the security advantage of scale-to-zero beyond cost savings?
## Since Cloud Run only starts to run when someone access it and not when it's idle, while with a VM, it's always running and could be risky in the way of who may have access/

## Q4: The OIDC workflow replaced the SSH key secrets from Weeks 3–5. What attack surface was eliminated?
## Since there was authentication in the background, there was no sensitive keys to be exposed.