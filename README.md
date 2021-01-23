# tekton-jinja
A tekton task for rendering jinja2 templates

### Use in your Kubernetes or OpenShift project

Install the `tekton-jinja` task into your namespace
```sh
$ kubectl apply -f https://raw.githubusercontent.com/erikerlandson/tekton-jinja/main/task-tekton-jinja.yaml
# or use 'oc apply ...' in OpenShift
```

Here is an example tekton pipeline that invokes the `tekton-jinja` task:

```yaml
apiVersion: tekton.dev/v1alpha1
kind: Pipeline
metadata:
  name: tekton-jinja-example
spec:
  workspaces:
    - name: jinja-data
  tasks:
    - name: render-job-yaml
      taskRef:
        kind: Task
        name: tekton-jinja
      params:
        - name: TEMPLATE_URLS
          value:
            - 'https://some.site.com/path/to/template1.abc'
            - 'https://another.site.com/path/to/template2.xyz'
        - name: TEMPLATE_VARS
          value:
            - jinja_var_1
            - value1
            - jinja_var_2
            - value2
      workspaces:
        - name: output
          workspace: jinja-data
```

By default, the `tekton-jinja` task uses container image `quay.io/erikerlandson/tekton-jinja`.
You can customize this image using the `TEKTON_JINJA_IMAGE` task parameter.
Refer to the task definition `task-tekton-jinja.yaml` in this directory for additional task parameters.

The `tekton-jinja` objects are currently using the tekton `tekton.dev/v1alpha1` API for compatibility with OpenShift Pipelines.
