<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github-production-user-asset-6210df.s3.amazonaws.com/658727/272340510-34957be5-7318-4473-8141-2751ca571c4f.png">
    <source media="(prefers-color-scheme: light)" srcset="https://github-production-user-asset-6210df.s3.amazonaws.com/658727/272340472-ad8d7a46-ef85-47ea-9129-d815206ed2f6.png">
    <img alt="Garden" src="https://github-production-user-asset-6210df.s3.amazonaws.com/658727/272340472-ad8d7a46-ef85-47ea-9129-d815206ed2f6.png">
  </picture>
</p>
<div align="center">
  <a href="https://garden.io/?utm_source=github-garden-coder">Website</a>
  <span>&nbsp;&nbsp;•&nbsp;&nbsp;</span>
  <a href="https://docs.garden.io/?utm_source=github-garden-coder">Docs</a>
  <span>&nbsp;&nbsp;•&nbsp;&nbsp;</span>
  <a href="https://github.com/garden-io/garden/tree/0.13.21/examples">Examples</a>
  <span>&nbsp;&nbsp;•&nbsp;&nbsp;</span>
  <a href="https://garden.io/blog/?utm_source=github-garden-coder">Blog</a>
  <span>&nbsp;&nbsp;•&nbsp;&nbsp;</span>
  <a href="https://go.garden.io/discord">Discord</a>
</div>

## Welcome to the Garden - Coder Example 👋

This repository contains an example project that demonstrates how to use Garden and Coder together.

- [Garden](https://docs.garden.io) is a DevOps automation platform that lets you spin-up production-like environments for development, testing and CI on demand.
- [Coder](https://coder.com/docs/v2/latest) is a self-hosted remote development platform that shifts software development from local machines to the cloud.

Together, they allow you to leverage the power of the cloud to **develop a system of any complexity fully remotely** with instant feedback that **feels like local**. See below for a quick demo video 👇

[Garden Coder demo video](https://github.com/garden-io/garden-coder-example/assets/5373776/20fc95fe-d358-4bbb-b3f0-c30ab55c5ef1)

## Usage

### Requirements

To try this example you'll need:

- [The Garden CLI](https://docs.garden.io/getting-started/quickstart#step-1-install-garden)
- A clone of this example project

You can quickly install the Garden CLI with:

```
curl -sL https://get.garden.io/install.sh | bash
```

### Step 1 — Log in to Garden

If you have the Garden CLI installed and a clone of this example project, log in from inside the example directory with:

```console
garden login
```

If this is your first time logging in, you'll be asked to sign up via GitHub.

### Step 2 — Create an access token

To simplify the next steps, we'll create a Garden access token and add it to the shell environment.

Open the [Garden dashboard users page](https://app.garden.io/users), then click the 'edit' icon next to your user name. This will open the user settings where you can create the access token. Copy the value and add it to your environment:

```console
export CODER_GARDEN_TOKEN=<your-garden-auth-token>
```

### Step 3 — Deploy Coder with Garden

Now you can deploy the project from the example directory with:

```console
garden deploy
```

This will deploy Coder to a Garden managed ephemeral cluster and create a workspace for your user.

You can now follow the link in your terminal (it'll look like `https://coder.<random-string>.preview.garden`) to open Coder in your browser and log in with the initial user credentials:

- Username: user@example.com
- Password: VeryStrongCoderPassword1234!

> [!IMPORTANT]
> Other people will not be able to access your Coder instance if deployed via Garden ephemeral clusters so this password is just a "formality".
>
> If deploying Coder to your _own_ environment, you should NEVER expose any passwords or credentials in plain text to git.
>
> Instead you can e.g. use Garden's secret management functionality to securely store sensitive values. Learn more [here](https://garden.io/plans)

### Step 4 — Deploy the example app from Coder

Once you've logged into your Coder instance, open the **"dev"** workspace that was created automatically when you ran the `garden deploy` command. The workspace will have Garden installed and a clone of the example app.

> [!TIP]
> Checkout the demo video above to see how to open the workspace.

Open a terminal in your workspace and change into the example app directory:

```console
cd quickstart-example
```

Then deploy the app with Garden by running the following from your Coder terminal:

```console
garden deploy --sync
```

And that's it! ✨

You've just deployed your fully remote development environment, and from that environment, a separate production-like environment for the actual example app.

The example is a voting app from Garden's [Quickstart example repository](https://github.com/garden-io/quickstart-example) and you can learn more about it there.

> [!TIP]
> The `--sync` flag enables live code syncing which means changes you make to the code will sync immediately to your running service.

## Under the hood

Garden enables you to codify your workflows, kind of like **a makefile for cloud native development**. When you run the `garden deploy` command it does the following:

- Spins up an ephemeral Kubernetes cluster (because that's the Garden plugin we're using in the [`project.garden.yml`](https://github.com/garden-io/garden-coder-example/blob/main/project.garden.yml) file)
- Installs a Postgres database and a Coder server (based on the config in the [`coder/garden.yml`](https://github.com/garden-io/garden-coder-example/blob/main/coder/garden.yml) file)
- Runs a handful of actions to initialize Coder and create the first workspace

Inside your Coder workspace you again use Garden, this time to deploy an example app. Here, Garden does the following:

- Deploys [the example app](https://github.com/garden-io/quickstart-example) to the same Kubernetes cluster but a different namespace (because the example is using the same Garden plugin)
- Enables live code syncing so that changes you make inside Coder update the example app immediately

## Who's this for?

Garden is typically used by teams with relatively large projects that run on Kubernetes and can't be easily developed or tested in a local environment.

Coder is typically used by teams who prefer writing their code in remote environments, e.g. for security reasons and/or to ensure development environments are consistent.

Using Garden and Coder together is therefore ideal for teams that want to address both concerns. That is, teams that need to be able to rapidly iterate on systems of any complexity without the source code ever touching their local machines.

## Using your own cluster

To make it easy to try things out, this example uses Garden managed ephemeral clusters—a zero-config sandbox environment that gets created on demand. To deploy Coder to your own Kubernetes cluster, follow [the instructions here](https://docs.garden.io/kubernetes-plugins/remote-k8s) and update the values in [`project.garden.yml`](https://github.com/garden-io/garden-coder-example/blob/main/project.garden.yml) file.

## Known Limitations

- The [Garden dashboard Live page](https://docs.garden.io/using-garden/dashboard#live-page) does not work when running Garden from within Coder. Other dashboard pages work as expected.
- The Deploy action isn't idempotent because the init step can't run twice. To just deploy Coder and skip initialization, run `garden deploy coder-server`.

## Further Reading

- [Garden documentation](https://docs.garden.io)
- [Coder documentation](https://coder.com/docs)
- [The example app](https://github.com/garden-io/quickstart-example)
