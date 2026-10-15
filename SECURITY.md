<!-- BEGIN MICROSOFT SECURITY.MD V0.0.9 BLOCK -->

## Security

Microsoft takes the security of our software products and services seriously, which includes all source code repositories managed through our GitHub organizations, which include [Microsoft](https://github.com/Microsoft), [Azure](https://github.com/Azure), [DotNet](https://github.com/dotnet), [AspNet](https://github.com/aspnet) and [Xamarin](https://github.com/xamarin).

If you believe you have found a security vulnerability in any Microsoft-owned repository that meets [Microsoft's definition of a security vulnerability](https://aka.ms/security.md/definition), please report it to us as described below.

## Reporting Security Issues

**Please do not report security vulnerabilities through public GitHub issues.**

Instead, please report them to the Microsoft Security Response Center (MSRC) at [https://msrc.microsoft.com/create-report](https://aka.ms/security.md/msrc/create-report).

If you prefer to submit without logging in, send email to [secure@microsoft.com](mailto:secure@microsoft.com).  If possible, encrypt your message with our PGP key; please download it from the [Microsoft Security Response Center PGP Key page](https://aka.ms/security.md/msrc/pgp).

You should receive a response within 24 hours. If for some reason you do not, please follow up via email to ensure we received your original message. Additional information can be found at [microsoft.com/msrc](https://www.microsoft.com/msrc).

Please include the requested information listed below (as much as you can provide) to help us better understand the nature and scope of the possible issue:

  * Type of issue (e.g. buffer overflow, SQL injection, cross-site scripting, etc.)
  * Full paths of source file(s) related to the manifestation of the issue
  * The location of the affected source code (tag/branch/commit or direct URL)
  * Any special configuration required to reproduce the issue
  * Step-by-step instructions to reproduce the issue
  * Proof-of-concept or exploit code (if possible)
  * Impact of the issue, including how an attacker might exploit the issue

This information will help us triage your report more quickly.

If you are reporting for a bug bounty, more complete reports can contribute to a higher bounty award. Please visit our [Microsoft Bug Bounty Program](https://aka.ms/security.md/msrc/bounty) page for more details about our active programs.

## Preferred Languages

We prefer all communications to be in English.

## Policy

Microsoft follows the principle of [Coordinated Vulnerability Disclosure](https://aka.ms/security.md/cvd).

<!-- END MICROSOFT SECURITY.MD BLOCK -->

## Internal CI runner configuration (administrator action required)

Repository workflow conditions are defense in depth, not a security boundary:
a pull request can change its workflow YAML. Before internal runners are enabled,
an organization administrator must restrict every runner group used by Skala to
selected workflows at these exact trusted refs:

- `microsoft/skala/.github/workflows/model-benchmark.yml@refs/heads/main`
- `microsoft/skala/.github/workflows/skala-test.yml@refs/heads/main`

Apply these restrictions to all three pools: `microsoft-skala-cpu-pool`,
`microsoft-skala-small-gpu-pool`, and `microsoft-skala-gpu-pool`. Restrict repository
access to the intended repository as well. Do not authorize PR merge refs,
wildcard refs, or other workflows that execute PR-controlled code. Protect `main`
with required review and restrict who can dispatch workflows on it.

The workflows run internal benchmarks and GPU tests only on trusted `main`
pushes or manual dispatches; PR CPU tests remain on GitHub-hosted runners.
Never use `pull_request_target` to check out or execute PR contents on these pools.

If the runner service cannot enforce selected workflows and refs, disable its
access to this public repository until equivalent isolation is available.
PR approval settings alone are not a replacement for runner isolation.
Validate the policy with a fork PR that attempts to remove job conditions and
target an internal pool: it must be denied runner access, not merely skipped by
the original YAML. Code changes alone do not establish that this configuration
has been deployed.
