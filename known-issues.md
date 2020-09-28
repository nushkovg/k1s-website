# Known Issues

- Prometheus can't scrape the metrics sometimes because the brower time and the local time are different on Linux. To fix this, use the following command:

  ```bash
  timedatectl set-local-rtc 1 --adjust-system-clock
  ```

- For some reason, if you close the browser tab and try to access the Kubernetes dashboard again, you might get an error. To fix this, just log out from the dashboard and use the same token to log in.

- Since Skaffold uses the `kubectl logs -f` command to follow logs from the services, if a service does not get logs in a time interval of 5-10 minutes, a timeout will happen and no logs will be shown for that particular service. This is a Skaffold bug and it's waiting for a fix.
