# Refinery Core

Refinery is a lightweight adapter framework designed primarily for extracting information snippets that can be paired into flashcards. It extracts data from various sources, e.g. highlight from iBooks and places a highlight+note pair in a self-hosted CouchDB instance. The information from the database can then be *refined* into a different format, e.g. a flashcard deck.

This repo contains core shared elements used the Refinery API repo. This covers stuff like Engines, `configProvider`, `IRecord` interface and `Record` class as well as SQL utils and general utility functions (including logger).

## User-defined Engines

To declare your own Engines, you can inherit from the `BaseEngine` class and implement `load()` method for ingress and `exportCallback()` for egress. Then, import your engines in `yourEngines.ts` in the base repository and pass the engine type to the exported list. Don't forget to run `tsc`!

Engines need to be defined by inheriting from the `BaseEngine` class and implementing a few details:
```typescript
import { BaseEngine } from 'refinery-core';
import { Record } from 'refinery-core';

export class FakeEngine extends BaseEngine {
  
  static descriptor = 'fake'         // Human-readable description for the logger (debugging)

  static arg = 'fake';               // CLI arg and API route ID

  static hasIngress = false;         // set to true if data is importable
  static hasEgress = true;           // set to true if data is exportable

  async load(): Promise<string[]> {  // implement this function for import
    let pr: Promise<string[]> = new Promise<string>((resolve, reject) => {});
    return pr;
  }

  exportCallback(output: string, records: Array<Record>, flipped: boolean) {
    return new Array<string>();     // implement this function for export
  }

}

export default FakeEngine;
```