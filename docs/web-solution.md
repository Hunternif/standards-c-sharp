# Web solution structure

This describes the standard abstract solution structure preferred by Agoda for a web-based project. Each box represents one project in the solution. Use common sense and adapt to your specific use-case.

Prefer to organize code primarily by functional area, rather than architectural layer (see examples below).

![Web solution structure](https://drive.google.com/uc?id=1XPy--wohqCpiioi2oOz9IaerMoHlWA_h)

## Presentation project(s)

### Folder structure

![Example folder structure](https://drive.google.com/uc?id=1HbIuJvfOOwTDzsindOHU9Sst9dr7Riw-)

### View models / DTOs

- Should be dumb and free of business logic, but if trivial may contain:
    - Purely presentational logic (eg. formatting). Refactor into a presentational service as necessary.
    - Mapping logic, eg `MyViewModel.From(MyModel model)`. Refactor into a separate model builder as necessary.

### Views

- Should be dumb and free of business logic.
- May contain presentational logic.

### Controllers

- Should be thin wrappers around services, existing purely as a bridge between HTTP and the service layer.
- Should be free of business logic.
- Should accept and return View models / DTOs when communicating with the outside world.
- Should accept and return Models when communicating internally.

### Presentational helpers

- Static classes with pure methods that implement more complex presentational logic.

## Service project

Should know nothing about the web / HTTP.

### Folder structure

![Example folder structure](https://drive.google.com/uc?id=17Z1K6g5RA5eHzoT7QjiIxWfjLlzNnpW4)

### Models

- An abstract representation of part of our domain.

### Business services

- Implements business logic and builds models.
- May aggregate data from other services if required.
- Should take at least one non-pure dependency, else should be converted to a helper. If any methods can be made static, consider moving them to a helper.
- Consider skipping the service entirely if it is only a thin wrapper around a repo or client library.

### Business helpers

- Static classes with pure methods that implement business logic.
- Prefer helpers over services for simplicity and to reduce constructor dependencies.

### Repo interfaces

- Should be defined here to make it easy to swap out repository implementations.

## Repository project

- Repo implementations.
- Can be combined with the Service project for convenience, or broken into a separate project to enforce correct chain of dependencies.
- May not be needed at all if you are only consuming backend services through client libraries.