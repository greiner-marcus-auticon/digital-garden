# Digital garden

This is a digital garden, a collection of notes, thoughts, and ideas that are still in the process of being formed.

## Prerequisites

Ensure you have the following installed:

- Ruby
- Bundler
- Jekyll

## Usage

Use the repository as a template, and clone it to your local machine:

```bash
git clone
```

Navigate to the project directory:

```bash
cd digital-garden
```

Install the dependencies:

```bash
bundle install
```

Run the site:

```bash
bundle exec jekyll serve
```

The site will be available at `http://localhost:4000`.

## Notes

Place your notes in the `_notes` directory.

## Running locally

To run the site locally, run the following command:

```bash
bundle exec jekyll serve
```

The site will be available at `http://localhost:4000`.

## Publishing

To publish the site, push the changes to the `main` branch.

The workflow in `.github/workflows/deploy.yml` will run, and
GitHub Pages will automatically build and deploy the site.

## About

This digital garden is based on [Digital garden Jekyll template](https://github.com/maximevaillancourt/digital-garden-jekyll-template) by [Maxime Vaillancourt](https://github.com/maximevaillancourt).

## License

Source code is available under the [MIT license](LICENSE.md).
