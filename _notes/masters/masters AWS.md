[gpu instance pricing](https://aws-pricing.com/gpu.html)
[a different site for price comparisons](https://cloudprice.net/aws/ec2)

[how do gpus work?](https://timdettmers.com/2023/01/30/which-gpu-for-deep-learning/)

got the T4's at around 0.5-0.7$ per hour
say training takes 72 hours, that's 36-50.4$ => 133.2 - 186.48 PLN
current training is like 3.5 days => 84 hours, oh

I should probs just check empirically
got the T4 running
[instances console](https://eu-central-1.console.aws.amazon.com/ec2/home?region=eu-central-1#Instances:)
serving tensorboard at `localhost:6007`

will have storage issues most likely, the training dir weight is pretty much only the best + last checkpoint though, with events being e.g. 300M for 500 epochs of Graz

address - keeps changing
- [x] connect to the AWS instance through a tailscale network