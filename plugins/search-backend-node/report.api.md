## API Report File for "@backstage/plugin-search-backend-node"

> Do not edit this file. It is a report generated by [API Extractor](https://api-extractor.com/).

```ts
import { BackstageCredentials } from '@backstage/backend-plugin-api';
import { Config } from '@backstage/config';
import { DocumentCollatorFactory } from '@backstage/plugin-search-common';
import { DocumentDecoratorFactory } from '@backstage/plugin-search-common';
import { DocumentTypeInfo } from '@backstage/plugin-search-common';
import { IndexableDocument } from '@backstage/plugin-search-common';
import { IndexableResultSet } from '@backstage/plugin-search-common';
import { LoggerService } from '@backstage/backend-plugin-api';
import { default as lunr_2 } from 'lunr';
import { Permission } from '@backstage/plugin-permission-common';
import { Readable } from 'stream';
import { SchedulerServiceTaskFunction } from '@backstage/backend-plugin-api';
import { SchedulerServiceTaskRunner } from '@backstage/backend-plugin-api';
import { SearchQuery } from '@backstage/plugin-search-common';
import { Transform } from 'stream';
import { UrlReaderService } from '@backstage/backend-plugin-api';
import { Writable } from 'stream';

// @public
export abstract class BatchSearchEngineIndexer extends Writable {
  constructor(options: BatchSearchEngineOptions);
  abstract finalize(): Promise<void>;
  abstract index(documents: IndexableDocument[]): Promise<void>;
  abstract initialize(): Promise<void>;
}

// @public
export type BatchSearchEngineOptions = {
  batchSize: number;
};

// @public
export type ConcreteLunrQuery = {
  lunrQueryBuilder: lunr_2.Index.QueryBuilder;
  documentTypes?: string[];
  pageSize: number;
};

// @public
export abstract class DecoratorBase extends Transform {
  constructor();
  abstract decorate(
    document: IndexableDocument,
  ): Promise<IndexableDocument | IndexableDocument[] | undefined>;
  abstract finalize(): Promise<void>;
  abstract initialize(): Promise<void>;
}

// @public
export class IndexBuilder {
  constructor(options: IndexBuilderOptions);
  addCollator(options: RegisterCollatorParameters): void;
  addDecorator(options: RegisterDecoratorParameters): void;
  build(): Promise<{
    scheduler: Scheduler;
  }>;
  getDocumentTypes(): Record<string, DocumentTypeInfo>;
  getSearchEngine(): SearchEngine;
}

// @public
export type IndexBuilderOptions = {
  searchEngine: SearchEngine;
  logger: LoggerService;
};

// @public
export type LunrQueryTranslator = (query: SearchQuery) => ConcreteLunrQuery;

// @public
export class LunrSearchEngine implements SearchEngine {
  constructor(options: { logger: LoggerService });
  // (undocumented)
  protected docStore: Record<string, IndexableDocument>;
  // (undocumented)
  getIndexer(type: string): Promise<LunrSearchEngineIndexer>;
  // (undocumented)
  protected highlightPostTag: string;
  // (undocumented)
  protected highlightPreTag: string;
  // (undocumented)
  protected logger: LoggerService;
  // (undocumented)
  protected lunrIndices: Record<string, lunr_2.Index>;
  // (undocumented)
  query(query: SearchQuery): Promise<IndexableResultSet>;
  // (undocumented)
  setTranslator(translator: LunrQueryTranslator): void;
  // (undocumented)
  protected translator: QueryTranslator;
}

// @public
export class LunrSearchEngineIndexer extends BatchSearchEngineIndexer {
  constructor();
  // (undocumented)
  buildIndex(): lunr_2.Index;
  // (undocumented)
  finalize(): Promise<void>;
  // (undocumented)
  getDocumentStore(): Record<string, IndexableDocument>;
  // (undocumented)
  index(documents: IndexableDocument[]): Promise<void>;
  // (undocumented)
  initialize(): Promise<void>;
}

// @public
export class MissingIndexError extends Error {
  constructor(message?: string, cause?: Error | unknown);
  readonly cause?: Error | undefined;
}

// @public
export class NewlineDelimitedJsonCollatorFactory
  implements DocumentCollatorFactory
{
  static fromConfig(
    _config: Config,
    options: NewlineDelimitedJsonCollatorFactoryOptions,
  ): NewlineDelimitedJsonCollatorFactory;
  // (undocumented)
  getCollator(): Promise<Readable>;
  // (undocumented)
  readonly type: string;
  // (undocumented)
  readonly visibilityPermission: Permission | undefined;
}

// @public
export type NewlineDelimitedJsonCollatorFactoryOptions = {
  type: string;
  searchPattern: string;
  reader: UrlReaderService;
  logger: LoggerService;
  visibilityPermission?: Permission;
};

// @public
export type QueryRequestOptions =
  | {
      token?: string;
    }
  | {
      credentials: BackstageCredentials;
    };

// @public
export type QueryTranslator = (query: SearchQuery) => unknown;

// @public
export interface RegisterCollatorParameters {
  factory: DocumentCollatorFactory;
  schedule: SchedulerServiceTaskRunner;
}

// @public
export interface RegisterDecoratorParameters {
  factory: DocumentDecoratorFactory;
}

// @public
export class Scheduler {
  constructor(options: { logger: LoggerService });
  addToSchedule(options: ScheduleTaskParameters): void;
  start(): void;
  stop(): void;
}

// @public
export type ScheduleTaskParameters = {
  id: string;
  task: SchedulerServiceTaskFunction;
  scheduledRunner: SchedulerServiceTaskRunner;
};

// @public
export interface SearchEngine {
  getIndexer(type: string): Promise<Writable>;
  query(
    query: SearchQuery,
    options?: QueryRequestOptions,
  ): Promise<IndexableResultSet>;
  setTranslator(translator: QueryTranslator): void;
}

// @public
export class TestPipeline {
  execute(): Promise<TestPipelineResult>;
  static fromCollator(collator: Readable): TestPipeline;
  static fromDecorator(decorator: Transform): TestPipeline;
  static fromIndexer(indexer: Writable): TestPipeline;
  withCollator(collator: Readable): this;
  withDecorator(decorator: Transform): this;
  withDocuments(documents: IndexableDocument[]): TestPipeline;
  withIndexer(indexer: Writable): this;
  // @deprecated
  static withSubject(subject: Readable | Transform | Writable): TestPipeline;
}

// @public
export type TestPipelineResult = {
  error: unknown;
  documents: IndexableDocument[];
};
```
