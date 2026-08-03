# Configuration Files

Instead of hardcoding values like:

```python
batch_size = 32
learning_rate = 0.001
```

I can place them inside a YAML configuration file.

Example:

```yaml
logging:
  level: INFO
```

The program then reads those values from the configuration file.

This makes experiments easier because I can change configuration without editing the source code.