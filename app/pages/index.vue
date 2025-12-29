<script setup lang="ts">
// AWS Aurora Postgres pricing - editable
const auroraPricing = reactive({
  // Provisioned instances with Standard and I/O-Optimized pricing
  instances: [
    // Standard Instances - Current Generation
    {
      name: "db.t4g.medium",
      vCPU: 2,
      ram: 4,
      pricePerHourStandard: 0.114,
      pricePerHourIOOptimized: 0.148,
    },
    {
      name: "db.t4g.large",
      vCPU: 2,
      ram: 8,
      pricePerHourStandard: 0.229,
      pricePerHourIOOptimized: 0.298,
    },
    {
      name: "db.t3.medium",
      vCPU: 2,
      ram: 4,
      pricePerHourStandard: 0.126,
      pricePerHourIOOptimized: 0.164,
    },
    {
      name: "db.t3.large",
      vCPU: 2,
      ram: 8,
      pricePerHourStandard: 0.252,
      pricePerHourIOOptimized: 0.328,
    },
    // Optimized Reads Instances - Current Generation
    {
      name: "r6gd.xlarge",
      vCPU: 4,
      ram: 32,
      pricePerHourStandard: 0.751,
      pricePerHourIOOptimized: 0.976,
    },
    {
      name: "r6gd.2xlarge",
      vCPU: 8,
      ram: 64,
      pricePerHourStandard: 1.502,
      pricePerHourIOOptimized: 1.953,
    },
    {
      name: "r6gd.4xlarge",
      vCPU: 16,
      ram: 128,
      pricePerHourStandard: 3.005,
      pricePerHourIOOptimized: 3.907,
    },
    {
      name: "r6gd.8xlarge",
      vCPU: 32,
      ram: 256,
      pricePerHourStandard: 6.01,
      pricePerHourIOOptimized: 7.813,
    },
    {
      name: "r6gd.12xlarge",
      vCPU: 48,
      ram: 384,
      pricePerHourStandard: 9.014,
      pricePerHourIOOptimized: 11.718,
    },
    {
      name: "r6gd.16xlarge",
      vCPU: 64,
      ram: 512,
      pricePerHourStandard: 12.019,
      pricePerHourIOOptimized: 15.625,
    },
    {
      name: "r6id.24xlarge",
      vCPU: 96,
      ram: 768,
      pricePerHourStandard: 20.16,
      pricePerHourIOOptimized: 26.208,
    },
    {
      name: "r6id.32xlarge",
      vCPU: 128,
      ram: 1024,
      pricePerHourStandard: 26.88,
      pricePerHourIOOptimized: 34.944,
    },
    // Memory Optimized Instances - Current Generation
    {
      name: "db.r8g.large",
      vCPU: 2,
      ram: 16,
      pricePerHourStandard: 0.333,
      pricePerHourIOOptimized: 0.433,
    },
    {
      name: "db.r8g.xlarge",
      vCPU: 4,
      ram: 32,
      pricePerHourStandard: 0.666,
      pricePerHourIOOptimized: 0.866,
    },
    {
      name: "db.r8g.2xlarge",
      vCPU: 8,
      ram: 64,
      pricePerHourStandard: 1.332,
      pricePerHourIOOptimized: 1.732,
    },
    {
      name: "db.r8g.4xlarge",
      vCPU: 16,
      ram: 128,
      pricePerHourStandard: 2.664,
      pricePerHourIOOptimized: 3.464,
    },
    {
      name: "db.r8g.8xlarge",
      vCPU: 32,
      ram: 256,
      pricePerHourStandard: 5.328,
      pricePerHourIOOptimized: 6.928,
    },
    {
      name: "db.r8g.12xlarge",
      vCPU: 48,
      ram: 384,
      pricePerHourStandard: 7.992,
      pricePerHourIOOptimized: 10.392,
    },
    {
      name: "db.r8g.16xlarge",
      vCPU: 64,
      ram: 512,
      pricePerHourStandard: 10.656,
      pricePerHourIOOptimized: 13.856,
    },
    {
      name: "db.r8g.24xlarge",
      vCPU: 96,
      ram: 768,
      pricePerHourStandard: 15.984,
      pricePerHourIOOptimized: 20.784,
    },
    {
      name: "db.r8g.48xlarge",
      vCPU: 192,
      ram: 1536,
      pricePerHourStandard: 31.968,
      pricePerHourIOOptimized: 41.568,
    },
    {
      name: "db.r7g.large",
      vCPU: 2,
      ram: 16,
      pricePerHourStandard: 0.333,
      pricePerHourIOOptimized: 0.433,
    },
    {
      name: "db.r7g.xlarge",
      vCPU: 4,
      ram: 32,
      pricePerHourStandard: 0.665,
      pricePerHourIOOptimized: 0.865,
    },
    {
      name: "db.r7g.2xlarge",
      vCPU: 8,
      ram: 64,
      pricePerHourStandard: 1.331,
      pricePerHourIOOptimized: 1.73,
    },
    {
      name: "db.r7g.4xlarge",
      vCPU: 16,
      ram: 128,
      pricePerHourStandard: 2.662,
      pricePerHourIOOptimized: 3.461,
    },
    {
      name: "db.r7g.8xlarge",
      vCPU: 32,
      ram: 256,
      pricePerHourStandard: 5.324,
      pricePerHourIOOptimized: 6.921,
    },
    {
      name: "db.r7g.12xlarge",
      vCPU: 48,
      ram: 384,
      pricePerHourStandard: 7.985,
      pricePerHourIOOptimized: 10.381,
    },
    {
      name: "db.r7g.16xlarge",
      vCPU: 64,
      ram: 512,
      pricePerHourStandard: 10.647,
      pricePerHourIOOptimized: 13.841,
    },
    {
      name: "db.r7i.large",
      vCPU: 2,
      ram: 16,
      pricePerHourStandard: 0.35,
      pricePerHourIOOptimized: 0.455,
    },
    {
      name: "db.r7i.xlarge",
      vCPU: 4,
      ram: 32,
      pricePerHourStandard: 0.7,
      pricePerHourIOOptimized: 0.91,
    },
    {
      name: "db.r7i.2xlarge",
      vCPU: 8,
      ram: 64,
      pricePerHourStandard: 1.4,
      pricePerHourIOOptimized: 1.82,
    },
    {
      name: "db.r7i.4xlarge",
      vCPU: 16,
      ram: 128,
      pricePerHourStandard: 2.8,
      pricePerHourIOOptimized: 3.64,
    },
    {
      name: "db.r7i.8xlarge",
      vCPU: 32,
      ram: 256,
      pricePerHourStandard: 5.6,
      pricePerHourIOOptimized: 7.28,
    },
    {
      name: "db.r7i.12xlarge",
      vCPU: 48,
      ram: 384,
      pricePerHourStandard: 8.4,
      pricePerHourIOOptimized: 10.92,
    },
    {
      name: "db.r7i.16xlarge",
      vCPU: 64,
      ram: 512,
      pricePerHourStandard: 11.2,
      pricePerHourIOOptimized: 14.56,
    },
    {
      name: "db.r7i.24xlarge",
      vCPU: 96,
      ram: 768,
      pricePerHourStandard: 16.8,
      pricePerHourIOOptimized: 21.84,
    },
    {
      name: "db.r7i.48xlarge",
      vCPU: 192,
      ram: 1536,
      pricePerHourStandard: 33.6,
      pricePerHourIOOptimized: 43.68,
    },
    {
      name: "db.r6g.large",
      vCPU: 2,
      ram: 16,
      pricePerHourStandard: 0.313,
      pricePerHourIOOptimized: 0.407,
    },
    {
      name: "db.r6g.xlarge",
      vCPU: 4,
      ram: 32,
      pricePerHourStandard: 0.627,
      pricePerHourIOOptimized: 0.815,
    },
    {
      name: "db.r6g.2xlarge",
      vCPU: 8,
      ram: 64,
      pricePerHourStandard: 1.253,
      pricePerHourIOOptimized: 1.629,
    },
    {
      name: "db.r6g.4xlarge",
      vCPU: 16,
      ram: 128,
      pricePerHourStandard: 2.506,
      pricePerHourIOOptimized: 3.258,
    },
    {
      name: "db.r6g.8xlarge",
      vCPU: 32,
      ram: 256,
      pricePerHourStandard: 5.012,
      pricePerHourIOOptimized: 6.516,
    },
    {
      name: "db.r6g.12xlarge",
      vCPU: 48,
      ram: 384,
      pricePerHourStandard: 7.518,
      pricePerHourIOOptimized: 9.773,
    },
    {
      name: "db.r6g.16xlarge",
      vCPU: 64,
      ram: 512,
      pricePerHourStandard: 10.024,
      pricePerHourIOOptimized: 13.031,
    },
    {
      name: "db.r6i.large",
      vCPU: 2,
      ram: 16,
      pricePerHourStandard: 0.35,
      pricePerHourIOOptimized: 0.455,
    },
    {
      name: "db.r6i.xlarge",
      vCPU: 4,
      ram: 32,
      pricePerHourStandard: 0.7,
      pricePerHourIOOptimized: 0.91,
    },
    {
      name: "db.r6i.2xlarge",
      vCPU: 8,
      ram: 64,
      pricePerHourStandard: 1.4,
      pricePerHourIOOptimized: 1.82,
    },
    {
      name: "db.r6i.4xlarge",
      vCPU: 16,
      ram: 128,
      pricePerHourStandard: 2.8,
      pricePerHourIOOptimized: 3.64,
    },
    {
      name: "db.r6i.8xlarge",
      vCPU: 32,
      ram: 256,
      pricePerHourStandard: 5.6,
      pricePerHourIOOptimized: 7.28,
    },
    {
      name: "db.r6i.12xlarge",
      vCPU: 48,
      ram: 384,
      pricePerHourStandard: 8.4,
      pricePerHourIOOptimized: 10.92,
    },
    {
      name: "db.r6i.16xlarge",
      vCPU: 64,
      ram: 512,
      pricePerHourStandard: 11.2,
      pricePerHourIOOptimized: 14.56,
    },
    {
      name: "db.r6i.24xlarge",
      vCPU: 96,
      ram: 768,
      pricePerHourStandard: 16.8,
      pricePerHourIOOptimized: 21.84,
    },
    {
      name: "db.r6i.32xlarge",
      vCPU: 128,
      ram: 1024,
      pricePerHourStandard: 22.4,
      pricePerHourIOOptimized: 29.12,
    },
    {
      name: "db.r5.large",
      vCPU: 2,
      ram: 16,
      pricePerHourStandard: 0.35,
      pricePerHourIOOptimized: 0.455,
    },
    {
      name: "db.r5.xlarge",
      vCPU: 4,
      ram: 32,
      pricePerHourStandard: 0.7,
      pricePerHourIOOptimized: 0.91,
    },
    {
      name: "db.r5.2xlarge",
      vCPU: 8,
      ram: 64,
      pricePerHourStandard: 1.4,
      pricePerHourIOOptimized: 1.82,
    },
    {
      name: "db.r5.4xlarge",
      vCPU: 16,
      ram: 128,
      pricePerHourStandard: 2.8,
      pricePerHourIOOptimized: 3.64,
    },
    {
      name: "db.r5.8xlarge",
      vCPU: 32,
      ram: 256,
      pricePerHourStandard: 5.6,
      pricePerHourIOOptimized: 7.28,
    },
    {
      name: "db.r5.12xlarge",
      vCPU: 48,
      ram: 384,
      pricePerHourStandard: 8.4,
      pricePerHourIOOptimized: 10.92,
    },
    {
      name: "db.r5.16xlarge",
      vCPU: 64,
      ram: 512,
      pricePerHourStandard: 11.2,
      pricePerHourIOOptimized: 14.56,
    },
    {
      name: "db.r5.24xlarge",
      vCPU: 96,
      ram: 768,
      pricePerHourStandard: 16.8,
      pricePerHourIOOptimized: 21.84,
    },
  ],
  // Provisioned pricing
  storagePerGB: 0.1,
  ioPerMillion: 0.2,
  backupPerGB: 0.021,
  dataTransferPerGB: 0.09,
  includedDataTransfer: 1,
  // Serverless pricing (Singapore region)
  serverless: {
    standard: {
      acuPerHour: 0.2,
      storagePerGB: 0.11,
      ioPerMillion: 0.22,
    },
    ioOptimized: {
      acuPerHour: 0.26,
      storagePerGB: 0.248,
      ioPerMillion: 0, // Included
    },
  },
});

// Neon DB pricing - editable
const neonPricing = reactive({
  tiers: {
    free: {
      computeUnitPerHour: 0, // Free tier: 100 CU-hours included
      includedCUHours: 100,
      storagePerGB: 0, // Free: 0.5 GB included
      includedStorage: 0.5,
      pitrStoragePerGB: 0, // Free: Limited window/size
      dataTransferPerGB: 0,
      includedDataTransfer: 5,
      minimumMonthly: 0,
    },
    launch: {
      computeUnitPerHour: 0.106, // $0.106 per CU-hour
      includedCUHours: 0,
      storagePerGB: 0.35, // $0.35 per GB-month
      includedStorage: 0,
      pitrStoragePerGB: 0.2, // $0.20 per GB-month
      dataTransferPerGB: 0.1, // $0.10 per GB after included
      includedDataTransfer: 100, // 100 GB included
      minimumMonthly: 5, // $5 minimum monthly
    },
    scale: {
      computeUnitPerHour: 0.222, // $0.222 per CU-hour
      includedCUHours: 0,
      storagePerGB: 0.35, // $0.35 per GB-month
      includedStorage: 0,
      pitrStoragePerGB: 0.2, // $0.20 per GB-month
      dataTransferPerGB: 0.1, // $0.10 per GB after included
      includedDataTransfer: 100, // 100 GB included
      minimumMonthly: 5, // $5 minimum monthly
    },
  },
});

// PlanetScale pricing - editable
const planetscalePricing = reactive({
  clusters: [
    { name: "PS-5", price: 15, vCPU: 0.0625, ram: 0.5, ha: true },
    { name: "PS-10", price: 47, vCPU: 0.125, ram: 1, ha: true },
    { name: "PS-20", price: 71, vCPU: 0.25, ram: 2, ha: true },
    { name: "PS-40", price: 119, vCPU: 0.5, ram: 4, ha: true },
    { name: "PS-80", price: 215, vCPU: 1, ram: 8, ha: true },
    { name: "PS-160", price: 419, vCPU: 2, ram: 16, ha: true },
    { name: "PS-320", price: 839, vCPU: 4, ram: 32, ha: true },
    { name: "PS-640", price: 1679, vCPU: 8, ram: 64, ha: true },
  ],
  storagePerGB: 0.45, // Additional storage after 10 GB included
  includedStorage: 10,
  dataTransferPerGB: 0.096,
  includedDataTransfer: 100,
  branchPerMonth: 0, // Branching included in HA plans
});

// Digital Ocean PostgreSQL pricing - editable
const digitalOceanPricing = reactive({
  cpuOptions: {
    basicRegular: {
      instances: [
        {
          name: "1-1-10",
          ram: 1,
          vCPU: 1,
          pricePerHour: 0.02254,
          pricePerMonth: 15.15,
          storageMin: 10,
          storageMax: 30,
        },
        {
          name: "1-2-30",
          ram: 2,
          vCPU: 1,
          pricePerHour: 0.04531,
          pricePerMonth: 30.45,
          storageMin: 30,
          storageMax: 60,
        },
        {
          name: "2-4-60",
          ram: 4,
          vCPU: 2,
          pricePerHour: 0.09063,
          pricePerMonth: 60.9,
          storageMin: 60,
          storageMax: 120,
        },
        {
          name: "4-8-140",
          ram: 8,
          vCPU: 4,
          pricePerHour: 0.1817,
          pricePerMonth: 122.1,
          storageMin: 140,
          storageMax: 280,
        },
        {
          name: "6-16-290",
          ram: 16,
          vCPU: 6,
          pricePerHour: 0.36362,
          pricePerMonth: 244.35,
          storageMin: 290,
          storageMax: 580,
        },
      ],
      instancesWith1Node: [
        {
          name: "1-2-30",
          ram: 2,
          vCPU: 1,
          pricePerHour: 0.09062,
          pricePerMonth: 60.9,
          storageMin: 30,
          storageMax: 60,
        },
        {
          name: "2-4-60",
          ram: 4,
          vCPU: 2,
          pricePerHour: 0.18125,
          pricePerMonth: 121.8,
          storageMin: 60,
          storageMax: 120,
        },
        {
          name: "4-8-140",
          ram: 8,
          vCPU: 4,
          pricePerHour: 0.36339,
          pricePerMonth: 244.2,
          storageMin: 140,
          storageMax: 280,
        },
        {
          name: "6-16-290",
          ram: 16,
          vCPU: 6,
          pricePerHour: 0.72723,
          pricePerMonth: 488.7,
          storageMin: 290,
          storageMax: 580,
        },
      ],
      instancesWith2Nodes: [
        {
          name: "1-2-30",
          ram: 2,
          vCPU: 1,
          pricePerHour: 0.13594,
          pricePerMonth: 91.35,
          storageMin: 30,
          storageMax: 60,
        },
        {
          name: "2-4-60",
          ram: 4,
          vCPU: 2,
          pricePerHour: 0.27188,
          pricePerMonth: 182.7,
          storageMin: 60,
          storageMax: 120,
        },
        {
          name: "4-8-140",
          ram: 8,
          vCPU: 4,
          pricePerHour: 0.54509,
          pricePerMonth: 366.3,
          storageMin: 140,
          storageMax: 280,
        },
        {
          name: "6-16-290",
          ram: 16,
          vCPU: 6,
          pricePerHour: 1.09085,
          pricePerMonth: 733.05,
          storageMin: 290,
          storageMax: 580,
        },
      ],
    },
    basicPremiumIntel: {
      instances: [
        {
          name: "1-1-20",
          ram: 1,
          vCPU: 1,
          pricePerHour: 0.03021,
          pricePerMonth: 20.3,
          storageMin: 20,
          storageMax: 40,
        },
        {
          name: "1-2-50",
          ram: 2,
          vCPU: 1,
          pricePerHour: 0.06064,
          pricePerMonth: 40.75,
          storageMin: 50,
          storageMax: 100,
        },
        {
          name: "2-4-80",
          ram: 4,
          vCPU: 2,
          pricePerHour: 0.12083,
          pricePerMonth: 81.2,
          storageMin: 80,
          storageMax: 160,
        },
        {
          name: "2-8-100",
          ram: 8,
          vCPU: 2,
          pricePerHour: 0.16443,
          pricePerMonth: 110.5,
          storageMin: 100,
          storageMax: 200,
        },
        {
          name: "4-8-160",
          ram: 8,
          vCPU: 4,
          pricePerHour: 0.24167,
          pricePerMonth: 162.4,
          storageMin: 160,
          storageMax: 320,
        },
        {
          name: "4-16-200",
          ram: 16,
          vCPU: 4,
          pricePerHour: 0.32887,
          pricePerMonth: 221.0,
          storageMin: 200,
          storageMax: 400,
        },
        {
          name: "8-32-400",
          ram: 32,
          vCPU: 8,
          pricePerHour: 0.65774,
          pricePerMonth: 442.0,
          storageMin: 400,
          storageMax: 800,
        },
        {
          name: "16-64-800",
          ram: 64,
          vCPU: 16,
          pricePerHour: 1.31548,
          pricePerMonth: 884.0,
          storageMin: 800,
          storageMax: 1600,
        },
      ],
      instancesWith1Node: [
        {
          name: "1-2-50",
          ram: 2,
          vCPU: 1,
          pricePerHour: 0.12128,
          pricePerMonth: 81.5,
          storageMin: 50,
          storageMax: 100,
        },
        {
          name: "2-4-80",
          ram: 4,
          vCPU: 2,
          pricePerHour: 0.24167,
          pricePerMonth: 162.4,
          storageMin: 80,
          storageMax: 160,
        },
        {
          name: "2-8-100",
          ram: 8,
          vCPU: 2,
          pricePerHour: 0.32887,
          pricePerMonth: 221.0,
          storageMin: 100,
          storageMax: 200,
        },
        {
          name: "4-8-160",
          ram: 8,
          vCPU: 4,
          pricePerHour: 0.48333,
          pricePerMonth: 324.8,
          storageMin: 160,
          storageMax: 320,
        },
        {
          name: "4-16-200",
          ram: 16,
          vCPU: 4,
          pricePerHour: 0.65774,
          pricePerMonth: 442.0,
          storageMin: 200,
          storageMax: 400,
        },
        {
          name: "8-32-400",
          ram: 32,
          vCPU: 8,
          pricePerHour: 1.31548,
          pricePerMonth: 884.0,
          storageMin: 400,
          storageMax: 800,
        },
        {
          name: "16-64-800",
          ram: 64,
          vCPU: 16,
          pricePerHour: 2.63095,
          pricePerMonth: 1768.0,
          storageMin: 800,
          storageMax: 1600,
        },
      ],
      instancesWith2Nodes: [
        {
          name: "1-2-50",
          ram: 2,
          vCPU: 1,
          pricePerHour: 0.18192,
          pricePerMonth: 122.25,
          storageMin: 50,
          storageMax: 100,
        },
        {
          name: "2-4-80",
          ram: 4,
          vCPU: 2,
          pricePerHour: 0.3625,
          pricePerMonth: 243.6,
          storageMin: 80,
          storageMax: 160,
        },
        {
          name: "2-8-100",
          ram: 8,
          vCPU: 2,
          pricePerHour: 0.4933,
          pricePerMonth: 331.5,
          storageMin: 100,
          storageMax: 200,
        },
        {
          name: "4-8-160",
          ram: 8,
          vCPU: 4,
          pricePerHour: 0.725,
          pricePerMonth: 487.2,
          storageMin: 160,
          storageMax: 320,
        },
        {
          name: "4-16-200",
          ram: 16,
          vCPU: 4,
          pricePerHour: 0.98661,
          pricePerMonth: 663.0,
          storageMin: 200,
          storageMax: 400,
        },
        {
          name: "8-32-400",
          ram: 32,
          vCPU: 8,
          pricePerHour: 1.97321,
          pricePerMonth: 1326.0,
          storageMin: 400,
          storageMax: 800,
        },
        {
          name: "16-64-800",
          ram: 64,
          vCPU: 16,
          pricePerHour: 3.94643,
          pricePerMonth: 2652.0,
          storageMin: 800,
          storageMax: 1600,
        },
      ],
    },
    basicPremiumAMD: {
      instances: [
        {
          name: "1-1-15",
          ram: 1,
          vCPU: 1,
          pricePerHour: 0.02712,
          pricePerMonth: 18.23,
          storageMin: 15,
          storageMax: 30,
        },
        {
          name: "1-2-40",
          ram: 2,
          vCPU: 1,
          pricePerHour: 0.05446,
          pricePerMonth: 36.6,
          storageMin: 40,
          storageMax: 80,
        },
        {
          name: "2-4-60",
          ram: 4,
          vCPU: 2,
          pricePerHour: 0.10848,
          pricePerMonth: 72.9,
          storageMin: 60,
          storageMax: 120,
        },
        {
          name: "2-8-80",
          ram: 8,
          vCPU: 2,
          pricePerHour: 0.14762,
          pricePerMonth: 99.2,
          storageMin: 80,
          storageMax: 160,
        },
        {
          name: "4-8-140",
          ram: 8,
          vCPU: 4,
          pricePerHour: 0.21741,
          pricePerMonth: 146.1,
          storageMin: 140,
          storageMax: 280,
        },
        {
          name: "4-16-170",
          ram: 16,
          vCPU: 4,
          pricePerHour: 0.29546,
          pricePerMonth: 198.55,
          storageMin: 170,
          storageMax: 340,
        },
        {
          name: "8-32-340",
          ram: 32,
          vCPU: 8,
          pricePerHour: 0.59092,
          pricePerMonth: 397.1,
          storageMin: 340,
          storageMax: 680,
        },
        {
          name: "16-64-680",
          ram: 64,
          vCPU: 16,
          pricePerHour: 1.16399,
          pricePerMonth: 782.2,
          storageMin: 680,
          storageMax: 1360,
        },
      ],
      instancesWith1Node: [
        {
          name: "1-2-40",
          ram: 2,
          vCPU: 1,
          pricePerHour: 0.10893,
          pricePerMonth: 73.2,
          storageMin: 40,
          storageMax: 80,
        },
        {
          name: "2-4-60",
          ram: 4,
          vCPU: 2,
          pricePerHour: 0.21696,
          pricePerMonth: 145.8,
          storageMin: 60,
          storageMax: 120,
        },
        {
          name: "2-8-80",
          ram: 8,
          vCPU: 2,
          pricePerHour: 0.29524,
          pricePerMonth: 198.4,
          storageMin: 80,
          storageMax: 160,
        },
        {
          name: "4-8-140",
          ram: 8,
          vCPU: 4,
          pricePerHour: 0.43482,
          pricePerMonth: 292.2,
          storageMin: 140,
          storageMax: 280,
        },
        {
          name: "4-16-170",
          ram: 16,
          vCPU: 4,
          pricePerHour: 0.59092,
          pricePerMonth: 397.1,
          storageMin: 170,
          storageMax: 340,
        },
        {
          name: "8-32-340",
          ram: 32,
          vCPU: 8,
          pricePerHour: 1.18185,
          pricePerMonth: 794.2,
          storageMin: 340,
          storageMax: 680,
        },
        {
          name: "16-64-680",
          ram: 64,
          vCPU: 16,
          pricePerHour: 2.32798,
          pricePerMonth: 1564.4,
          storageMin: 680,
          storageMax: 1360,
        },
      ],
      instancesWith2Nodes: [
        {
          name: "1-2-40",
          ram: 2,
          vCPU: 1,
          pricePerHour: 0.16339,
          pricePerMonth: 109.8,
          storageMin: 40,
          storageMax: 80,
        },
        {
          name: "2-4-60",
          ram: 4,
          vCPU: 2,
          pricePerHour: 0.32545,
          pricePerMonth: 218.7,
          storageMin: 60,
          storageMax: 120,
        },
        {
          name: "2-8-80",
          ram: 8,
          vCPU: 2,
          pricePerHour: 0.44286,
          pricePerMonth: 297.6,
          storageMin: 80,
          storageMax: 160,
        },
        {
          name: "4-8-140",
          ram: 8,
          vCPU: 4,
          pricePerHour: 0.65223,
          pricePerMonth: 438.3,
          storageMin: 140,
          storageMax: 280,
        },
        {
          name: "4-16-170",
          ram: 16,
          vCPU: 4,
          pricePerHour: 0.88638,
          pricePerMonth: 595.65,
          storageMin: 170,
          storageMax: 340,
        },
        {
          name: "8-32-340",
          ram: 32,
          vCPU: 8,
          pricePerHour: 1.77277,
          pricePerMonth: 1191.3,
          storageMin: 340,
          storageMax: 680,
        },
        {
          name: "16-64-680",
          ram: 64,
          vCPU: 16,
          pricePerHour: 3.49196,
          pricePerMonth: 2346.6,
          storageMin: 680,
          storageMax: 1360,
        },
      ],
    },
    generalPurpose: {
      instances: [
        {
          name: "2-8-30",
          ram: 8,
          vCPU: 2,
          pricePerHour: 0.16882,
          pricePerMonth: 113.45,
          storageMin: 30,
          storageMax: 60,
        },
        {
          name: "4-16-70",
          ram: 16,
          vCPU: 4,
          pricePerHour: 0.33787,
          pricePerMonth: 227.05,
          storageMin: 70,
          storageMax: 140,
        },
        {
          name: "8-32-160",
          ram: 32,
          vCPU: 8,
          pricePerHour: 0.66429,
          pricePerMonth: 446.4,
          storageMin: 160,
          storageMax: 320,
        },
        {
          name: "16-64-360",
          ram: 64,
          vCPU: 16,
          pricePerHour: 1.33839,
          pricePerMonth: 899.4,
          storageMin: 360,
          storageMax: 720,
        },
        {
          name: "32-128-760",
          ram: 128,
          vCPU: 32,
          pricePerHour: 2.71637,
          pricePerMonth: 1825.4,
          storageMin: 760,
          storageMax: 1520,
        },
        {
          name: "40-160-960",
          ram: 160,
          vCPU: 40,
          pricePerHour: 3.39792,
          pricePerMonth: 2283.4,
          storageMin: 960,
          storageMax: 1920,
        },
      ],
      instancesWith1Node: [
        {
          name: "2-8-30",
          ram: 8,
          vCPU: 2,
          pricePerHour: 0.33765,
          pricePerMonth: 226.9,
          storageMin: 30,
          storageMax: 60,
        },
        {
          name: "4-16-70",
          ram: 16,
          vCPU: 4,
          pricePerHour: 0.67574,
          pricePerMonth: 454.1,
          storageMin: 70,
          storageMax: 140,
        },
        {
          name: "8-32-160",
          ram: 32,
          vCPU: 8,
          pricePerHour: 1.32857,
          pricePerMonth: 892.8,
          storageMin: 160,
          storageMax: 320,
        },
        {
          name: "16-64-360",
          ram: 64,
          vCPU: 16,
          pricePerHour: 2.67679,
          pricePerMonth: 1798.8,
          storageMin: 360,
          storageMax: 720,
        },
        {
          name: "32-128-760",
          ram: 128,
          vCPU: 32,
          pricePerHour: 5.43274,
          pricePerMonth: 3650.8,
          storageMin: 760,
          storageMax: 1520,
        },
        {
          name: "40-160-960",
          ram: 160,
          vCPU: 40,
          pricePerHour: 6.79583,
          pricePerMonth: 4566.8,
          storageMin: 960,
          storageMax: 1920,
        },
      ],
      instancesWith2Nodes: [
        {
          name: "2-8-30",
          ram: 8,
          vCPU: 2,
          pricePerHour: 0.50647,
          pricePerMonth: 340.35,
          storageMin: 30,
          storageMax: 60,
        },
        {
          name: "4-16-70",
          ram: 16,
          vCPU: 4,
          pricePerHour: 1.01362,
          pricePerMonth: 681.15,
          storageMin: 70,
          storageMax: 140,
        },
        {
          name: "8-32-160",
          ram: 32,
          vCPU: 8,
          pricePerHour: 1.99286,
          pricePerMonth: 1339.2,
          storageMin: 160,
          storageMax: 320,
        },
        {
          name: "16-64-360",
          ram: 64,
          vCPU: 16,
          pricePerHour: 4.01518,
          pricePerMonth: 2698.2,
          storageMin: 360,
          storageMax: 720,
        },
        {
          name: "32-128-760",
          ram: 128,
          vCPU: 32,
          pricePerHour: 8.14911,
          pricePerMonth: 5476.2,
          storageMin: 760,
          storageMax: 1520,
        },
        {
          name: "40-160-960",
          ram: 160,
          vCPU: 40,
          pricePerHour: 10.19375,
          pricePerMonth: 6850.2,
          storageMin: 960,
          storageMax: 1920,
        },
      ],
    },
    storageOptimized: {
      instances: [
        {
          name: "2-16-300",
          ram: 16,
          vCPU: 2,
          pricePerHour: 0.32217,
          pricePerMonth: 216.5,
          storageMin: 300,
          storageMax: 600,
        },
        {
          name: "4-32-600",
          ram: 32,
          vCPU: 4,
          pricePerHour: 0.64137,
          pricePerMonth: 431.0,
          storageMin: 600,
          storageMax: 1200,
        },
        {
          name: "8-64-1200",
          ram: 64,
          vCPU: 8,
          pricePerHour: 1.29167,
          pricePerMonth: 868.0,
          storageMin: 1200,
          storageMax: 2400,
        },
        {
          name: "16-128-2400",
          ram: 128,
          vCPU: 16,
          pricePerHour: 2.58036,
          pricePerMonth: 1734.0,
          storageMin: 2400,
          storageMax: 4800,
        },
        {
          name: "24-192-3600",
          ram: 192,
          vCPU: 24,
          pricePerHour: 3.87202,
          pricePerMonth: 2602.0,
          storageMin: 3600,
          storageMax: 7200,
        },
        {
          name: "32-256-4800",
          ram: 256,
          vCPU: 32,
          pricePerHour: 5.14583,
          pricePerMonth: 3458.0,
          storageMin: 4800,
          storageMax: 9600,
        },
      ],
      instancesWith1Node: [
        {
          name: "2-16-300",
          ram: 16,
          vCPU: 2,
          pricePerHour: 0.64435,
          pricePerMonth: 433.0,
          storageMin: 300,
          storageMax: 600,
        },
        {
          name: "4-32-600",
          ram: 32,
          vCPU: 4,
          pricePerHour: 1.28274,
          pricePerMonth: 862.0,
          storageMin: 600,
          storageMax: 1200,
        },
        {
          name: "8-64-1200",
          ram: 64,
          vCPU: 8,
          pricePerHour: 2.58333,
          pricePerMonth: 1736.0,
          storageMin: 1200,
          storageMax: 2400,
        },
        {
          name: "16-128-2400",
          ram: 128,
          vCPU: 16,
          pricePerHour: 5.16071,
          pricePerMonth: 3468.0,
          storageMin: 2400,
          storageMax: 4800,
        },
        {
          name: "24-192-3600",
          ram: 192,
          vCPU: 24,
          pricePerHour: 7.74405,
          pricePerMonth: 5204.0,
          storageMin: 3600,
          storageMax: 7200,
        },
        {
          name: "32-256-4800",
          ram: 256,
          vCPU: 32,
          pricePerHour: 10.29167,
          pricePerMonth: 6916.0,
          storageMin: 4800,
          storageMax: 9600,
        },
      ],
      instancesWith2Nodes: [
        {
          name: "2-16-300",
          ram: 16,
          vCPU: 2,
          pricePerHour: 0.96652,
          pricePerMonth: 649.5,
          storageMin: 300,
          storageMax: 600,
        },
        {
          name: "4-32-600",
          ram: 32,
          vCPU: 4,
          pricePerHour: 1.92411,
          pricePerMonth: 1293.0,
          storageMin: 600,
          storageMax: 1200,
        },
        {
          name: "8-64-1200",
          ram: 64,
          vCPU: 8,
          pricePerHour: 3.875,
          pricePerMonth: 2604.0,
          storageMin: 1200,
          storageMax: 2400,
        },
        {
          name: "16-128-2400",
          ram: 128,
          vCPU: 16,
          pricePerHour: 7.74107,
          pricePerMonth: 5202.0,
          storageMin: 2400,
          storageMax: 4800,
        },
        {
          name: "24-192-3600",
          ram: 192,
          vCPU: 24,
          pricePerHour: 11.61607,
          pricePerMonth: 7806.0,
          storageMin: 3600,
          storageMax: 7200,
        },
        {
          name: "32-256-4800",
          ram: 256,
          vCPU: 32,
          pricePerHour: 15.4375,
          pricePerMonth: 10374.0,
          storageMin: 4800,
          storageMax: 9600,
        },
      ],
    },
  },
  storagePerGB: 0.215, // $0.215 per GiB per month
  dataTransferPerGB: 0.01, // $0.01 per GiB for outbound (egress) traffic
  includedDataTransfer: 1000, // 1000 GiB included per month for Managed Databases
});

// Vercel Static IP pricing
const vercelStaticIPPricing = reactive({
  monthlyFee: 100, // $100/month per project
  privateDataTransferPerGB: 0.15, // $0.15/GB for private data transfer
});

// Input state
const inputs = reactive<{
  storageGB: number;
  dataTransferGB: number;
  vercelStaticIP: boolean;
  auroraType: "provisioned" | "serverless";
  auroraInstanceName: string;
  auroraProvisionedConfig: "standard" | "ioOptimized";
  auroraIORequests: number;
  auroraBackupGB: number;
  neonComputeUnits: number;
  neonPITRStorageGB: number;
  planetscaleClusterName: string;
  planetscaleBranches: number;
  auroraServerlessACU: number;
  auroraServerlessConfig: "standard" | "ioOptimized";
  neonTier: "free" | "launch" | "scale";
  digitalOceanInstanceName: string;
  digitalOceanCPUOption:
    | "basicRegular"
    | "basicPremiumIntel"
    | "basicPremiumAMD"
    | "generalPurpose"
    | "storageOptimized";
  digitalOceanAdditionalNodes: number;
  serverlessHoursPerMonth: number; // Hours per month for serverless databases (accounts for downtime)
}>({
  // Common inputs
  storageGB: 100,
  dataTransferGB: 50,
  vercelStaticIP: false,
  serverlessHoursPerMonth: 720, // Default: 24 * 30 = 720 hours (24/7)

  // AWS Aurora
  auroraType: "serverless",
  auroraInstanceName: "db.r5.large",
  auroraProvisionedConfig: "standard",
  auroraIORequests: 10, // millions per month
  auroraBackupGB: 50,
  // Aurora Serverless
  auroraServerlessACU: 2, // Average ACU capacity
  auroraServerlessConfig: "standard", // "standard" or "ioOptimized"

  // Neon DB
  neonTier: "scale",
  neonComputeUnits: 2,
  neonPITRStorageGB: 20,

  // PlanetScale
  planetscaleClusterName: "PS-10",
  planetscaleBranches: 1,

  // Digital Ocean
  digitalOceanInstanceName: "1-2-30",
  digitalOceanCPUOption: "basicRegular",
  digitalOceanAdditionalNodes: 0,
});

// Computed instance/cluster objects
const auroraInstance = computed(() => {
  const instance =
    auroraPricing.instances.find((i) => i.name === inputs.auroraInstanceName) ||
    auroraPricing.instances[0];
  if (!instance) return null;

  // Add computed pricePerHour based on selected config
  return {
    ...instance,
    pricePerHour:
      inputs.auroraProvisionedConfig === "ioOptimized"
        ? instance.pricePerHourIOOptimized
        : instance.pricePerHourStandard,
  };
});

const planetscaleCluster = computed(() => {
  return (
    planetscalePricing.clusters.find(
      (c) => c.name === inputs.planetscaleClusterName
    ) || planetscalePricing.clusters[1]
  );
});

const digitalOceanInstance = computed(() => {
  const cpuOption =
    digitalOceanPricing.cpuOptions[inputs.digitalOceanCPUOption];
  if (!cpuOption) {
    return null;
  }

  // Select instance list based on number of additional nodes
  let instanceList = cpuOption.instances || [];
  if (
    inputs.digitalOceanAdditionalNodes === 1 &&
    cpuOption.instancesWith1Node
  ) {
    instanceList = cpuOption.instancesWith1Node;
  } else if (
    inputs.digitalOceanAdditionalNodes === 2 &&
    cpuOption.instancesWith2Nodes
  ) {
    instanceList = cpuOption.instancesWith2Nodes;
  }

  if (!instanceList || instanceList.length === 0) {
    return null;
  }

  return (
    instanceList.find((i) => i.name === inputs.digitalOceanInstanceName) ||
    instanceList[0]
  );
});

// Watch for changes to CPU option or additional nodes and reset instance name
watch(
  [
    () => inputs.digitalOceanCPUOption,
    () => inputs.digitalOceanAdditionalNodes,
  ],
  () => {
    const cpuOption =
      digitalOceanPricing.cpuOptions[inputs.digitalOceanCPUOption];
    if (!cpuOption) return;

    // Select instance list based on number of additional nodes
    let instanceList = cpuOption.instances || [];
    if (
      inputs.digitalOceanAdditionalNodes === 1 &&
      (cpuOption as any).instancesWith1Node
    ) {
      instanceList = (cpuOption as any).instancesWith1Node;
    } else if (
      inputs.digitalOceanAdditionalNodes === 2 &&
      (cpuOption as any).instancesWith2Nodes
    ) {
      instanceList = (cpuOption as any).instancesWith2Nodes;
    }

    if (instanceList && instanceList.length > 0) {
      // Check if current instance name exists in the new list
      const currentInstanceExists = instanceList.some(
        (i: any) => i.name === inputs.digitalOceanInstanceName
      );
      // If not, reset to first available instance
      if (!currentInstanceExists) {
        inputs.digitalOceanInstanceName = instanceList[0]?.name || "";
      }
    }
  }
);

// Vercel Static IP cost calculation
const vercelStaticIPCost = computed(() => {
  if (!inputs.vercelStaticIP) {
    return {
      monthlyFee: 0,
      privateDataTransfer: 0,
      total: 0,
    };
  }

  const monthlyFee = vercelStaticIPPricing.monthlyFee;
  // Private data transfer cost (assuming all data transfer goes through private network when Static IP is enabled)
  const privateDataTransferCost =
    inputs.dataTransferGB * vercelStaticIPPricing.privateDataTransferPerGB;

  return {
    monthlyFee,
    privateDataTransfer: privateDataTransferCost,
    total: monthlyFee + privateDataTransferCost,
  };
});

// Cost calculations
const auroraCost = computed(() => {
  if (inputs.auroraType === "serverless") {
    // Aurora Serverless pricing
    const config =
      inputs.auroraServerlessConfig === "ioOptimized"
        ? auroraPricing.serverless.ioOptimized
        : auroraPricing.serverless.standard;

    // ACU cost: billed per second, but we calculate per hour then multiply by hours in month
    // Using serverlessHoursPerMonth to account for downtime (defaults to 720 = 24/7)
    const acuCost =
      inputs.auroraServerlessACU *
      config.acuPerHour *
      inputs.serverlessHoursPerMonth;
    const storageCost = inputs.storageGB * config.storagePerGB;
    const ioCost = inputs.auroraIORequests * config.ioPerMillion;
    const backupCost = inputs.auroraBackupGB * auroraPricing.backupPerGB;
    const dataTransferCost =
      Math.max(0, inputs.dataTransferGB - auroraPricing.includedDataTransfer) *
      auroraPricing.dataTransferPerGB;

    const baseTotal =
      acuCost + storageCost + ioCost + backupCost + dataTransferCost;
    return {
      acu: acuCost,
      storage: storageCost,
      io: ioCost,
      backup: backupCost,
      dataTransfer: dataTransferCost,
      vercelStaticIP: vercelStaticIPCost.value.total,
      total: baseTotal + vercelStaticIPCost.value.total,
    };
  } else {
    // Aurora Provisioned pricing
    const instance = auroraInstance.value;
    if (!instance) {
      return {
        instance: 0,
        storage: 0,
        io: 0,
        backup: 0,
        dataTransfer: 0,
        vercelStaticIP: vercelStaticIPCost.value.total,
        total: vercelStaticIPCost.value.total,
      };
    }
    const instanceCost = instance.pricePerHour * 24 * 30;
    const storageCost = inputs.storageGB * auroraPricing.storagePerGB;
    const ioCost = inputs.auroraIORequests * auroraPricing.ioPerMillion;
    const backupCost = inputs.auroraBackupGB * auroraPricing.backupPerGB;
    const dataTransferCost =
      Math.max(0, inputs.dataTransferGB - auroraPricing.includedDataTransfer) *
      auroraPricing.dataTransferPerGB;

    const baseTotal =
      instanceCost + storageCost + ioCost + backupCost + dataTransferCost;
    return {
      instance: instanceCost,
      storage: storageCost,
      io: ioCost,
      backup: backupCost,
      dataTransfer: dataTransferCost,
      vercelStaticIP: vercelStaticIPCost.value.total,
      total: baseTotal + vercelStaticIPCost.value.total,
    };
  }
});

const neonCost = computed(() => {
  const tier = neonPricing.tiers[inputs.neonTier];

  // Compute cost: calculate total CU-hours for the month, subtract included if any
  // Using serverlessHoursPerMonth to account for downtime (defaults to 720 = 24/7)
  const totalCUHours = inputs.neonComputeUnits * inputs.serverlessHoursPerMonth;
  const billableCUHours = Math.max(0, totalCUHours - tier.includedCUHours);
  const computeCost = billableCUHours * tier.computeUnitPerHour;

  // Storage cost: subtract included storage if any
  const billableStorage = Math.max(0, inputs.storageGB - tier.includedStorage);
  const storageCost = billableStorage * tier.storagePerGB;

  // PITR storage cost
  const pitrCost = inputs.neonPITRStorageGB * tier.pitrStoragePerGB;

  // Data transfer cost: subtract included if any
  const billableDataTransfer = Math.max(
    0,
    inputs.dataTransferGB - tier.includedDataTransfer
  );
  const dataTransferCost = billableDataTransfer * tier.dataTransferPerGB;

  const subtotal = computeCost + storageCost + pitrCost + dataTransferCost;
  const baseTotal = Math.max(subtotal, tier.minimumMonthly);

  return {
    compute: computeCost,
    storage: storageCost,
    pitr: pitrCost,
    dataTransfer: dataTransferCost,
    vercelStaticIP: vercelStaticIPCost.value.total,
    total: baseTotal + vercelStaticIPCost.value.total,
  };
});

const planetscaleCost = computed(() => {
  const cluster = planetscaleCluster.value!;
  const clusterCost = cluster.price;
  const storageCost =
    Math.max(0, inputs.storageGB - planetscalePricing.includedStorage) *
    planetscalePricing.storagePerGB;
  const dataTransferCost =
    Math.max(
      0,
      inputs.dataTransferGB - planetscalePricing.includedDataTransfer
    ) * planetscalePricing.dataTransferPerGB;
  const branchCost =
    Math.max(0, inputs.planetscaleBranches - 1) *
    planetscalePricing.branchPerMonth; // Branching included in HA plans

  const baseTotal = clusterCost + storageCost + dataTransferCost + branchCost;
  return {
    cluster: clusterCost,
    storage: storageCost,
    dataTransfer: dataTransferCost,
    branches: branchCost,
    vercelStaticIP: vercelStaticIPCost.value.total,
    total: baseTotal + vercelStaticIPCost.value.total,
  };
});

const digitalOceanCost = computed(() => {
  const instance = digitalOceanInstance.value;
  if (!instance) {
    return {
      instance: 0,
      storage: 0,
      dataTransfer: 0,
      vercelStaticIP: vercelStaticIPCost.value.total,
      total: vercelStaticIPCost.value.total,
    };
  }

  // Instance cost calculation
  // For base instances (0 additional nodes), multiply by totalNodes if nodes are added
  // For instancesWith1Node/instancesWith2Nodes, price already includes additional nodes
  let instanceCost = instance.pricePerMonth;
  const cpuOption =
    digitalOceanPricing.cpuOptions[inputs.digitalOceanCPUOption];
  if (cpuOption) {
    const isFrom1NodeList =
      inputs.digitalOceanAdditionalNodes === 1 &&
      cpuOption.instancesWith1Node?.some((i) => i.name === instance.name);
    const isFrom2NodeList =
      inputs.digitalOceanAdditionalNodes === 2 &&
      cpuOption.instancesWith2Nodes?.some((i) => i.name === instance.name);

    // If using base instances and have additional nodes, multiply
    if (
      !isFrom1NodeList &&
      !isFrom2NodeList &&
      inputs.digitalOceanAdditionalNodes > 0
    ) {
      instanceCost =
        instance.pricePerMonth * (1 + inputs.digitalOceanAdditionalNodes);
    }
    // Otherwise, price already includes the nodes (for 1Node/2Node lists) or is base price
  }

  // Storage cost: $0.215 per GiB per month
  // Note: Digital Ocean storage is separate from instance pricing
  const storageCost = inputs.storageGB * digitalOceanPricing.storagePerGB;

  // Data transfer cost: $0.01/GiB for outbound (egress) traffic
  // Inbound traffic is free, outbound has included allowance
  const billableDataTransfer = Math.max(
    0,
    inputs.dataTransferGB - digitalOceanPricing.includedDataTransfer
  );
  const dataTransferCost =
    billableDataTransfer * digitalOceanPricing.dataTransferPerGB;

  const baseTotal = instanceCost + storageCost + dataTransferCost;
  return {
    instance: instanceCost,
    storage: storageCost,
    dataTransfer: dataTransferCost,
    vercelStaticIP: vercelStaticIPCost.value.total,
    total: baseTotal + vercelStaticIPCost.value.total,
  };
});

const comparison = computed(() => {
  const auroraName =
    inputs.auroraType === "serverless"
      ? `AWS Aurora Postgres Serverless (${
          inputs.auroraServerlessConfig === "ioOptimized"
            ? "I/O-Optimized"
            : "Standard"
        })`
      : "AWS Aurora Postgres (Provisioned)";

  const neonName = `Neon DB (${
    inputs.neonTier.charAt(0).toUpperCase() + inputs.neonTier.slice(1)
  } Tier)`;

  const costs = [
    {
      name: auroraName,
      cost: auroraCost.value.total,
      breakdown: auroraCost.value,
    },
    { name: neonName, cost: neonCost.value.total, breakdown: neonCost.value },
    {
      name: "PlanetScale",
      cost: planetscaleCost.value.total,
      breakdown: planetscaleCost.value,
    },
    {
      name: "Digital Ocean PostgreSQL",
      cost: digitalOceanCost.value.total,
      breakdown: digitalOceanCost.value,
    },
  ];

  return costs.sort((a, b) => a.cost - b.cost);
});

const formatCurrency = (value: number) => {
  return new Intl.NumberFormat("en-US", {
    style: "currency",
    currency: "USD",
    minimumFractionDigits: 2,
    maximumFractionDigits: 2,
  }).format(value);
};
</script>

<template>
  <UContainer>
    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 my-8">
      <!-- Input Panel -->
      <div class="lg:col-span-1">
        <UCard>
          <template #header>
            <h2 class="text-xl font-semibold">Usage Parameters</h2>
          </template>

          <div class="space-y-6">
            <!-- Common Inputs -->
            <div>
              <h3 class="text-sm font-medium mb-4 text-muted">
                Common Requirements
              </h3>
              <div class="space-y-4">
                <UFormField label="Storage (GB)" name="storage">
                  <UInputNumber
                    v-model="inputs.storageGB"
                    :min="1"
                    :max="10000"
                    :step="1"
                  />
                </UFormField>

                <UFormField
                  label="Data Transfer (GB/month)"
                  name="dataTransfer"
                >
                  <UInputNumber
                    v-model="inputs.dataTransferGB"
                    :min="0"
                    :max="10000"
                    :step="1"
                  />
                </UFormField>

                <UFormField label="Vercel Static IP" name="vercelStaticIP">
                  <div class="flex items-center gap-2">
                    <UCheckbox v-model="inputs.vercelStaticIP" />
                    <span class="text-sm">Enable Vercel Static IP</span>
                  </div>
                  <template #hint>
                    $100/month + $0.15/GB for private data transfer (IP
                    allowlisting)
                  </template>
                </UFormField>

                <UFormField
                  label="Serverless Uptime (hours/month)"
                  name="serverlessHours"
                >
                  <UInputNumber
                    v-model="inputs.serverlessHoursPerMonth"
                    :min="1"
                    :max="744"
                    :step="1"
                  />
                  <template #hint>
                    Hours per month for serverless databases (Aurora Serverless,
                    Neon DB). Default: 720 hours (24/7). Adjust to account for
                    downtime or scale-to-zero periods.
                  </template>
                </UFormField>
              </div>
            </div>

            <USeparator />

            <!-- AWS Aurora -->
            <div class="pt-2">
              <h3 class="text-sm font-medium mb-4 text-muted">
                AWS Aurora Postgres
              </h3>
              <div class="space-y-4">
                <UFormField label="Aurora Type" name="auroraType">
                  <USelect
                    v-model="inputs.auroraType"
                    :items="[
                      { label: 'Provisioned', value: 'provisioned' },
                      { label: 'Serverless', value: 'serverless' },
                    ]"
                  />
                </UFormField>

                <!-- Provisioned Options -->
                <template v-if="inputs.auroraType === 'provisioned'">
                  <UFormField label="Instance Type" name="auroraInstance">
                    <USelect
                      v-model="inputs.auroraInstanceName"
                      :items="
                        auroraPricing.instances.map((i) => ({
                          label: `${i.name} (${i.vCPU} vCPU, ${i.ram}GB RAM)`,
                          value: i.name,
                        }))
                      "
                    />
                  </UFormField>

                  <UFormField
                    label="Configuration"
                    name="auroraProvisionedConfig"
                  >
                    <USelect
                      v-model="inputs.auroraProvisionedConfig"
                      :items="[
                        { label: 'Aurora Standard', value: 'standard' },
                        {
                          label: 'Aurora I/O-Optimized',
                          value: 'ioOptimized',
                        },
                      ]"
                    />
                  </UFormField>

                  <UFormField
                    label="I/O Requests (millions/month)"
                    name="auroraIO"
                  >
                    <UInputNumber
                      v-model="inputs.auroraIORequests"
                      :min="0"
                      :max="1000"
                      :step="0.1"
                    />
                    <template #hint>
                      I/O is included with I/O-Optimized configuration
                    </template>
                  </UFormField>
                </template>

                <!-- Serverless Options -->
                <template v-else>
                  <UFormField
                    label="Configuration"
                    name="auroraServerlessConfig"
                  >
                    <USelect
                      v-model="inputs.auroraServerlessConfig"
                      :items="[
                        { label: 'Aurora Standard', value: 'standard' },
                        {
                          label: 'Aurora I/O-Optimized',
                          value: 'ioOptimized',
                        },
                      ]"
                    />
                  </UFormField>

                  <UFormField
                    label="Average ACU Capacity"
                    name="auroraServerlessACU"
                  >
                    <UInputNumber
                      v-model="inputs.auroraServerlessACU"
                      :min="0.5"
                      :max="256"
                      :step="0.5"
                    />
                    <template #hint>
                      1 ACU ≈ 2 GiB memory. Billed per second.
                    </template>
                  </UFormField>

                  <UFormField
                    v-if="inputs.auroraServerlessConfig === 'standard'"
                    label="I/O Requests (millions/month)"
                    name="auroraIO"
                  >
                    <UInputNumber
                      v-model="inputs.auroraIORequests"
                      :min="0"
                      :max="1000"
                      :step="0.1"
                    />
                    <template #hint>
                      I/O is included with I/O-Optimized
                    </template>
                  </UFormField>
                </template>

                <UFormField label="Backup Storage (GB)" name="auroraBackup">
                  <UInputNumber
                    v-model="inputs.auroraBackupGB"
                    :min="0"
                    :max="10000"
                    :step="1"
                  />
                </UFormField>
              </div>
            </div>

            <USeparator />

            <!-- Neon DB -->
            <div class="pt-2">
              <h3 class="text-sm font-medium mb-4 text-muted">Neon DB</h3>
              <div class="space-y-4">
                <UFormField label="Tier" name="neonTier">
                  <USelect
                    v-model="inputs.neonTier"
                    :items="[
                      { label: 'Free', value: 'free' },
                      { label: 'Launch', value: 'launch' },
                      { label: 'Scale', value: 'scale' },
                    ]"
                  />
                </UFormField>

                <UFormField label="Compute Units" name="neonCU">
                  <UInputNumber
                    v-model="inputs.neonComputeUnits"
                    :min="1"
                    :max="100"
                    :step="1"
                  />
                  <template #hint>
                    {{
                      inputs.neonTier === "free"
                        ? "100 CU-hours included per project"
                        : inputs.neonTier === "launch"
                        ? `Each CU costs $${neonPricing.tiers.launch.computeUnitPerHour}/hour`
                        : `Each CU costs $${neonPricing.tiers.scale.computeUnitPerHour}/hour`
                    }}
                  </template>
                </UFormField>

                <UFormField label="PITR Storage (GB)" name="neonPITR">
                  <UInputNumber
                    v-model="inputs.neonPITRStorageGB"
                    :min="0"
                    :max="10000"
                    :step="1"
                  />
                </UFormField>
              </div>
            </div>

            <USeparator />

            <!-- PlanetScale -->
            <div class="pt-2">
              <h3 class="text-sm font-medium mb-4 text-muted">PlanetScale</h3>
              <div class="space-y-4">
                <UFormField label="Cluster Type" name="planetscaleCluster">
                  <USelect
                    v-model="inputs.planetscaleClusterName"
                    :items="
                      planetscalePricing.clusters.map((c) => ({
                        label: `${c.name} - $${c.price}/mo (${c.vCPU} vCPU, ${
                          c.ram
                        }GB RAM${c.ha ? ', HA' : ''})`,
                        value: c.name,
                      }))
                    "
                  />
                  <template #hint>
                    Select the cluster configuration that matches your workload
                  </template>
                </UFormField>

                <UFormField
                  label="Development Branches"
                  name="planetscaleBranches"
                >
                  <UInputNumber
                    v-model="inputs.planetscaleBranches"
                    :min="1"
                    :max="50"
                    :step="1"
                  />
                  <template #hint> Branching included in HA plans </template>
                </UFormField>
              </div>
            </div>

            <USeparator />

            <!-- Digital Ocean -->
            <div class="pt-2">
              <h3 class="text-sm font-medium mb-4 text-muted">
                Digital Ocean PostgreSQL
              </h3>
              <div class="space-y-4">
                <UFormField label="CPU Options" name="digitalOceanCPU">
                  <USelect
                    v-model="inputs.digitalOceanCPUOption"
                    :items="[
                      { label: 'Basic Regular', value: 'basicRegular' },
                      {
                        label: 'Basic Premium Intel',
                        value: 'basicPremiumIntel',
                      },
                      { label: 'Basic Premium AMD', value: 'basicPremiumAMD' },
                      { label: 'General Purpose', value: 'generalPurpose' },
                      { label: 'Storage Optimized', value: 'storageOptimized' },
                    ]"
                  />
                  <template #hint>
                    Select the CPU option for your database instance
                  </template>
                </UFormField>

                <UFormField label="Instance Type" name="digitalOceanInstance">
                  <USelect
                    v-model="inputs.digitalOceanInstanceName"
                    :items="
                      (() => {
                        const cpuOption =
                          digitalOceanPricing.cpuOptions[
                            inputs.digitalOceanCPUOption
                          ];
                        if (!cpuOption) return [];
                        let instanceList = cpuOption.instances || [];
                        if (
                          inputs.digitalOceanAdditionalNodes === 1 &&
                          cpuOption.instancesWith1Node
                        ) {
                          instanceList = cpuOption.instancesWith1Node;
                        } else if (
                          inputs.digitalOceanAdditionalNodes === 2 &&
                          cpuOption.instancesWith2Nodes
                        ) {
                          instanceList = cpuOption.instancesWith2Nodes;
                        }
                        return instanceList.map((i) => ({
                          label: `${i.ram} GiB RAM, ${i.vCPU} vCPU - $${i.pricePerMonth}/mo`,
                          value: i.name,
                        }));
                      })()
                    "
                  />
                  <template #hint>
                    Select the instance configuration that matches your workload
                  </template>
                </UFormField>

                <UFormField label="Additional Nodes" name="digitalOceanNodes">
                  <UInputNumber
                    v-model="inputs.digitalOceanAdditionalNodes"
                    :min="0"
                    :max="2"
                    :step="1"
                  />
                  <template #hint>
                    Add standby nodes for high availability (0-2 additional
                    nodes)
                  </template>
                </UFormField>
              </div>
            </div>
          </div>
        </UCard>

        <!-- Pricing Configuration -->
        <UCard class="mt-6">
          <template #header>
            <h2 class="text-xl font-semibold">Pricing Configuration</h2>
          </template>

          <div class="space-y-6">
            <!-- AWS Aurora Pricing -->
            <UCollapsible>
              <template #default="{ open }">
                <UButton
                  color="neutral"
                  variant="ghost"
                  class="w-full justify-between rounded-lg p-3"
                >
                  <span class="font-medium">AWS Aurora Postgres Pricing</span>
                  <UIcon
                    :name="
                      open ? 'i-lucide-chevron-up' : 'i-lucide-chevron-down'
                    "
                    class="w-4 h-4"
                  />
                </UButton>
              </template>
              <template #content>
                <div class="space-y-6 pt-4">
                  <!-- Provisioned Pricing (only show when Provisioned is selected) -->
                  <div v-if="inputs.auroraType === 'provisioned'">
                    <h4 class="text-sm font-medium mb-3">
                      Provisioned Pricing
                    </h4>
                    <div class="grid grid-cols-2 gap-4 mb-4">
                      <UFormField
                        label="Storage per GB ($)"
                        name="auroraStorage"
                      >
                        <UInputNumber
                          v-model="auroraPricing.storagePerGB"
                          :min="0"
                          :max="10"
                          :step="0.01"
                        />
                      </UFormField>
                      <UFormField label="I/O per Million ($)" name="auroraIO">
                        <UInputNumber
                          v-model="auroraPricing.ioPerMillion"
                          :min="0"
                          :max="10"
                          :step="0.01"
                        />
                      </UFormField>
                      <UFormField label="Backup per GB ($)" name="auroraBackup">
                        <UInputNumber
                          v-model="auroraPricing.backupPerGB"
                          :min="0"
                          :max="1"
                          :step="0.001"
                        />
                      </UFormField>
                      <UFormField
                        label="Data Transfer per GB ($)"
                        name="auroraDataTransfer"
                      >
                        <UInputNumber
                          v-model="auroraPricing.dataTransferPerGB"
                          :min="0"
                          :max="1"
                          :step="0.01"
                        />
                      </UFormField>
                      <UFormField
                        label="Included Data Transfer (GB)"
                        name="auroraIncludedDataTransfer"
                      >
                        <UInputNumber
                          v-model="auroraPricing.includedDataTransfer"
                          :min="0"
                          :max="1000"
                          :step="1"
                        />
                      </UFormField>
                    </div>
                    <div>
                      <h5 class="text-sm font-medium mb-2">Instance Pricing</h5>
                      <div class="space-y-3">
                        <div
                          v-for="(instance, idx) in auroraPricing.instances"
                          :key="instance.name"
                          class="p-3 border rounded-lg space-y-2"
                        >
                          <div class="font-medium">
                            {{ instance.name }} ({{ instance.vCPU }} vCPU,
                            {{ instance.ram }}GB RAM)
                          </div>
                          <div class="grid grid-cols-2 gap-2">
                            <UFormField
                              :label="`Standard Price/Hour ($)`"
                              :name="`aurora-instance-standard-${idx}`"
                            >
                              <UInputNumber
                                v-model="auroraPricing.instances[idx]!.pricePerHourStandard"
                                :min="0"
                                :max="100"
                                :step="0.001"
                              />
                            </UFormField>
                            <UFormField
                              :label="`I/O-Optimized Price/Hour ($)`"
                              :name="`aurora-instance-io-${idx}`"
                            >
                              <UInputNumber
                                v-model="auroraPricing.instances[idx]!.pricePerHourIOOptimized"
                                :min="0"
                                :max="100"
                                :step="0.001"
                              />
                            </UFormField>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>

                  <!-- Serverless Pricing -->
                  <div v-if="inputs.auroraType === 'serverless'">
                    <h4 class="text-sm font-medium mb-3">Serverless Pricing</h4>
                    <div class="grid grid-cols-2 gap-4 mb-4">
                      <UFormField label="Backup per GB ($)" name="auroraBackup">
                        <UInputNumber
                          v-model="auroraPricing.backupPerGB"
                          :min="0"
                          :max="1"
                          :step="0.001"
                        />
                      </UFormField>
                      <UFormField
                        label="Data Transfer per GB ($)"
                        name="auroraDataTransfer"
                      >
                        <UInputNumber
                          v-model="auroraPricing.dataTransferPerGB"
                          :min="0"
                          :max="1"
                          :step="0.01"
                        />
                      </UFormField>
                      <UFormField
                        label="Included Data Transfer (GB)"
                        name="auroraIncludedDataTransfer"
                      >
                        <UInputNumber
                          v-model="auroraPricing.includedDataTransfer"
                          :min="0"
                          :max="1000"
                          :step="1"
                        />
                      </UFormField>
                    </div>
                    <div class="space-y-4">
                      <div>
                        <h5 class="text-xs font-medium mb-2 text-muted">
                          Aurora Standard
                        </h5>
                        <div class="grid grid-cols-2 gap-4">
                          <UFormField
                            label="ACU per Hour ($)"
                            name="auroraServerlessStandardACU"
                          >
                            <UInputNumber
                              v-model="
                                auroraPricing.serverless.standard.acuPerHour
                              "
                              :min="0"
                              :max="10"
                              :step="0.01"
                            />
                          </UFormField>
                          <UFormField
                            label="Storage per GB ($)"
                            name="auroraServerlessStandardStorage"
                          >
                            <UInputNumber
                              v-model="
                                auroraPricing.serverless.standard.storagePerGB
                              "
                              :min="0"
                              :max="10"
                              :step="0.01"
                            />
                          </UFormField>
                          <UFormField
                            label="I/O per Million ($)"
                            name="auroraServerlessStandardIO"
                          >
                            <UInputNumber
                              v-model="
                                auroraPricing.serverless.standard.ioPerMillion
                              "
                              :min="0"
                              :max="10"
                              :step="0.01"
                            />
                          </UFormField>
                        </div>
                      </div>
                      <div>
                        <h5 class="text-xs font-medium mb-2 text-muted">
                          Aurora I/O-Optimized
                        </h5>
                        <div class="grid grid-cols-2 gap-4">
                          <UFormField
                            label="ACU per Hour ($)"
                            name="auroraServerlessIOACU"
                          >
                            <UInputNumber
                              v-model="
                                auroraPricing.serverless.ioOptimized.acuPerHour
                              "
                              :min="0"
                              :max="10"
                              :step="0.01"
                            />
                          </UFormField>
                          <UFormField
                            label="Storage per GB ($)"
                            name="auroraServerlessIOStorage"
                          >
                            <UInputNumber
                              v-model="
                                auroraPricing.serverless.ioOptimized
                                  .storagePerGB
                              "
                              :min="0"
                              :max="10"
                              :step="0.01"
                            />
                          </UFormField>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </template>
            </UCollapsible>

            <!-- Neon DB Pricing -->
            <UCollapsible>
              <template #default="{ open }">
                <UButton
                  color="neutral"
                  variant="ghost"
                  class="w-full justify-between rounded-lg p-3"
                >
                  <span class="font-medium">Neon DB Pricing</span>
                  <UIcon
                    :name="
                      open ? 'i-lucide-chevron-up' : 'i-lucide-chevron-down'
                    "
                    class="w-4 h-4"
                  />
                </UButton>
              </template>
              <template #content>
                <div class="space-y-6 pt-4">
                  <!-- Free Tier -->
                  <div>
                    <h4 class="text-sm font-medium mb-3">Free Tier</h4>
                    <div class="grid grid-cols-2 gap-4">
                      <UFormField
                        label="Included CU-hours"
                        name="neonFreeCUHours"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.free.includedCUHours"
                          :min="0"
                          :max="1000"
                          :step="1"
                        />
                      </UFormField>
                      <UFormField
                        label="Included Storage (GB)"
                        name="neonFreeStorage"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.free.includedStorage"
                          :min="0"
                          :max="10"
                          :step="0.1"
                        />
                      </UFormField>
                      <UFormField
                        label="Included Data Transfer (GB)"
                        name="neonFreeDataTransfer"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.free.includedDataTransfer"
                          :min="0"
                          :max="100"
                          :step="1"
                        />
                      </UFormField>
                    </div>
                  </div>

                  <!-- Launch Tier -->
                  <div>
                    <h4 class="text-sm font-medium mb-3">Launch Tier</h4>
                    <div class="grid grid-cols-2 gap-4">
                      <UFormField
                        label="Compute Unit per Hour ($)"
                        name="neonLaunchCU"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.launch.computeUnitPerHour"
                          :min="0"
                          :max="10"
                          :step="0.001"
                        />
                      </UFormField>
                      <UFormField
                        label="Storage per GB ($)"
                        name="neonLaunchStorage"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.launch.storagePerGB"
                          :min="0"
                          :max="10"
                          :step="0.01"
                        />
                      </UFormField>
                      <UFormField
                        label="PITR Storage per GB ($)"
                        name="neonLaunchPITR"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.launch.pitrStoragePerGB"
                          :min="0"
                          :max="10"
                          :step="0.01"
                        />
                      </UFormField>
                      <UFormField
                        label="Data Transfer per GB ($)"
                        name="neonLaunchDataTransfer"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.launch.dataTransferPerGB"
                          :min="0"
                          :max="1"
                          :step="0.01"
                        />
                      </UFormField>
                      <UFormField
                        label="Included Data Transfer (GB)"
                        name="neonLaunchIncludedDataTransfer"
                      >
                        <UInputNumber
                          v-model="
                            neonPricing.tiers.launch.includedDataTransfer
                          "
                          :min="0"
                          :max="1000"
                          :step="1"
                        />
                      </UFormField>
                      <UFormField
                        label="Minimum Monthly ($)"
                        name="neonLaunchMinimum"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.launch.minimumMonthly"
                          :min="0"
                          :max="100"
                          :step="1"
                        />
                      </UFormField>
                    </div>
                  </div>

                  <!-- Scale Tier -->
                  <div>
                    <h4 class="text-sm font-medium mb-3">Scale Tier</h4>
                    <div class="grid grid-cols-2 gap-4">
                      <UFormField
                        label="Compute Unit per Hour ($)"
                        name="neonScaleCU"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.scale.computeUnitPerHour"
                          :min="0"
                          :max="10"
                          :step="0.001"
                        />
                      </UFormField>
                      <UFormField
                        label="Storage per GB ($)"
                        name="neonScaleStorage"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.scale.storagePerGB"
                          :min="0"
                          :max="10"
                          :step="0.01"
                        />
                      </UFormField>
                      <UFormField
                        label="PITR Storage per GB ($)"
                        name="neonScalePITR"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.scale.pitrStoragePerGB"
                          :min="0"
                          :max="10"
                          :step="0.01"
                        />
                      </UFormField>
                      <UFormField
                        label="Data Transfer per GB ($)"
                        name="neonScaleDataTransfer"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.scale.dataTransferPerGB"
                          :min="0"
                          :max="1"
                          :step="0.01"
                        />
                      </UFormField>
                      <UFormField
                        label="Included Data Transfer (GB)"
                        name="neonScaleIncludedDataTransfer"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.scale.includedDataTransfer"
                          :min="0"
                          :max="1000"
                          :step="1"
                        />
                      </UFormField>
                      <UFormField
                        label="Minimum Monthly ($)"
                        name="neonScaleMinimum"
                      >
                        <UInputNumber
                          v-model="neonPricing.tiers.scale.minimumMonthly"
                          :min="0"
                          :max="100"
                          :step="1"
                        />
                      </UFormField>
                    </div>
                  </div>
                </div>
              </template>
            </UCollapsible>

            <!-- PlanetScale Pricing -->
            <UCollapsible>
              <template #default="{ open }">
                <UButton
                  color="neutral"
                  variant="ghost"
                  class="w-full justify-between rounded-lg p-3"
                >
                  <span class="font-medium">PlanetScale Pricing</span>
                  <UIcon
                    :name="
                      open ? 'i-lucide-chevron-up' : 'i-lucide-chevron-down'
                    "
                    class="w-4 h-4"
                  />
                </UButton>
              </template>
              <template #content>
                <div class="space-y-4 pt-4">
                  <div class="grid grid-cols-2 gap-4">
                    <UFormField
                      label="Storage per GB ($)"
                      name="planetscaleStorage"
                    >
                      <UInputNumber
                        v-model="planetscalePricing.storagePerGB"
                        :min="0"
                        :max="10"
                        :step="0.01"
                      />
                    </UFormField>
                    <UFormField
                      label="Included Storage (GB)"
                      name="planetscaleIncludedStorage"
                    >
                      <UInputNumber
                        v-model="planetscalePricing.includedStorage"
                        :min="0"
                        :max="1000"
                        :step="1"
                      />
                    </UFormField>
                    <UFormField
                      label="Data Transfer per GB ($)"
                      name="planetscaleDataTransfer"
                    >
                      <UInputNumber
                        v-model="planetscalePricing.dataTransferPerGB"
                        :min="0"
                        :max="1"
                        :step="0.01"
                      />
                    </UFormField>
                    <UFormField
                      label="Included Data Transfer (GB)"
                      name="planetscaleIncludedDataTransfer"
                    >
                      <UInputNumber
                        v-model="planetscalePricing.includedDataTransfer"
                        :min="0"
                        :max="10000"
                        :step="1"
                      />
                    </UFormField>
                    <UFormField
                      label="Branch per Month ($)"
                      name="planetscaleBranch"
                    >
                      <UInputNumber
                        v-model="planetscalePricing.branchPerMonth"
                        :min="0"
                        :max="100"
                        :step="1"
                      />
                    </UFormField>
                  </div>
                  <div>
                    <h4 class="text-sm font-medium mb-2">Cluster Pricing</h4>
                    <div class="space-y-3">
                      <div
                        v-for="(cluster, idx) in planetscalePricing.clusters"
                        :key="cluster.name"
                        class="p-3 border rounded-lg space-y-2"
                      >
                        <div class="font-medium">{{ cluster.name }}</div>
                        <UFormField
                          :label="`${cluster.name} Price/Month ($)`"
                          :name="`planetscale-cluster-${idx}`"
                        >
                          <UInputNumber
                            v-model="planetscalePricing.clusters[idx]!.price"
                            :min="0"
                            :max="10000"
                            :step="1"
                          />
                        </UFormField>
                      </div>
                    </div>
                  </div>
                </div>
              </template>
            </UCollapsible>

            <!-- Digital Ocean Pricing -->
            <UCollapsible>
              <template #default="{ open }">
                <UButton
                  color="neutral"
                  variant="ghost"
                  class="w-full justify-between rounded-lg p-3"
                >
                  <span class="font-medium"
                    >Digital Ocean PostgreSQL Pricing</span
                  >
                  <UIcon
                    :name="
                      open ? 'i-lucide-chevron-up' : 'i-lucide-chevron-down'
                    "
                    class="w-4 h-4"
                  />
                </UButton>
              </template>
              <template #content>
                <div class="space-y-4 pt-4">
                  <div class="grid grid-cols-2 gap-4">
                    <UFormField
                      label="Storage per GiB ($)"
                      name="digitalOceanStorage"
                    >
                      <UInputNumber
                        v-model="digitalOceanPricing.storagePerGB"
                        :min="0"
                        :max="10"
                        :step="0.001"
                      />
                    </UFormField>
                    <UFormField
                      label="Data Transfer per GiB ($)"
                      name="digitalOceanDataTransfer"
                    >
                      <UInputNumber
                        v-model="digitalOceanPricing.dataTransferPerGB"
                        :min="0"
                        :max="1"
                        :step="0.001"
                      />
                    </UFormField>
                    <UFormField
                      label="Included Data Transfer (GiB)"
                      name="digitalOceanIncludedDataTransfer"
                    >
                      <UInputNumber
                        v-model="digitalOceanPricing.includedDataTransfer"
                        :min="0"
                        :max="10000"
                        :step="1"
                      />
                    </UFormField>
                  </div>
                  <div>
                    <h4 class="text-sm font-medium mb-2">Instance Pricing</h4>
                    <div class="space-y-6">
                      <div
                        v-for="(
                          cpuOption, cpuKey
                        ) in digitalOceanPricing.cpuOptions"
                        :key="cpuKey"
                      >
                        <h5 class="text-xs font-medium mb-2 text-muted">
                          {{
                            cpuKey === "basicRegular"
                              ? "Basic Regular"
                              : cpuKey === "basicPremiumIntel"
                              ? "Basic Premium Intel"
                              : cpuKey === "basicPremiumAMD"
                              ? "Basic Premium AMD"
                              : cpuKey === "generalPurpose"
                              ? "General Purpose"
                              : "Storage Optimized"
                          }}
                        </h5>
                        <!-- Base instances (0 additional nodes) -->
                        <div
                          v-if="
                            cpuOption.instances &&
                            cpuOption.instances.length > 0
                          "
                        >
                          <h6 class="text-xs font-medium mb-2 text-muted">
                            Base Instances (0 additional nodes)
                          </h6>
                          <div class="space-y-3">
                            <div
                              v-for="(instance, idx) in cpuOption.instances"
                              :key="`${cpuKey}-base-${instance.name}`"
                              class="p-3 border rounded-lg space-y-2"
                            >
                              <div class="font-medium">
                                {{ instance.ram }} GiB RAM,
                                {{ instance.vCPU }} vCPU
                              </div>
                              <div class="grid grid-cols-2 gap-2">
                                <UFormField
                                  :label="`Price/Hour ($)`"
                                  :name="`digitalocean-${cpuKey}-base-hourly-${idx}`"
                                >
                                  <UInputNumber
                                    v-model="
                                      digitalOceanPricing.cpuOptions[cpuKey].instances[
                                        idx
                                      ]!.pricePerHour
                                    "
                                    :min="0"
                                    :max="10"
                                    :step="0.00001"
                                  />
                                </UFormField>
                                <UFormField
                                  :label="`Price/Month ($)`"
                                  :name="`digitalocean-${cpuKey}-base-monthly-${idx}`"
                                >
                                  <UInputNumber
                                    v-model="
                                      digitalOceanPricing.cpuOptions[cpuKey].instances[
                                        idx
                                      ]!.pricePerMonth
                                    "
                                    :min="0"
                                    :max="10000"
                                    :step="0.01"
                                  />
                                </UFormField>
                              </div>
                            </div>
                          </div>
                        </div>
                        <!-- Instances with 1 additional node -->
                        <div
                          v-if="
                            cpuOption.instancesWith1Node &&
                            cpuOption.instancesWith1Node.length > 0
                          "
                          class="mt-4"
                        >
                          <h6 class="text-xs font-medium mb-2 text-muted">
                            Instances with 1 additional node
                          </h6>
                          <div class="space-y-3">
                            <div
                              v-for="(
                                instance, idx
                              ) in cpuOption.instancesWith1Node"
                              :key="`${cpuKey}-1node-${instance.name}`"
                              class="p-3 border rounded-lg space-y-2"
                            >
                              <div class="font-medium">
                                {{ instance.ram }} GiB RAM,
                                {{ instance.vCPU }} vCPU
                              </div>
                              <div class="grid grid-cols-2 gap-2">
                                <UFormField
                                  :label="`Price/Hour ($)`"
                                  :name="`digitalocean-${cpuKey}-1node-hourly-${idx}`"
                                >
                                  <UInputNumber
                                    v-model="
                                      digitalOceanPricing.cpuOptions[cpuKey].instancesWith1Node[
                                        idx
                                      ]!.pricePerHour
                                    "
                                    :min="0"
                                    :max="10"
                                    :step="0.00001"
                                  />
                                </UFormField>
                                <UFormField
                                  :label="`Price/Month ($)`"
                                  :name="`digitalocean-${cpuKey}-1node-monthly-${idx}`"
                                >
                                  <UInputNumber
                                    v-model="
                                      digitalOceanPricing.cpuOptions[cpuKey].instancesWith1Node[
                                        idx
                                      ]!.pricePerMonth
                                    "
                                    :min="0"
                                    :max="10000"
                                    :step="0.01"
                                  />
                                </UFormField>
                              </div>
                            </div>
                          </div>
                        </div>
                        <!-- Instances with 2 additional nodes -->
                        <div
                          v-if="
                            cpuOption.instancesWith2Nodes &&
                            cpuOption.instancesWith2Nodes.length > 0
                          "
                          class="mt-4"
                        >
                          <h6 class="text-xs font-medium mb-2 text-muted">
                            Instances with 2 additional nodes
                          </h6>
                          <div class="space-y-3">
                            <div
                              v-for="(
                                instance, idx
                              ) in cpuOption.instancesWith2Nodes"
                              :key="`${cpuKey}-2nodes-${instance.name}`"
                              class="p-3 border rounded-lg space-y-2"
                            >
                              <div class="font-medium">
                                {{ instance.ram }} GiB RAM,
                                {{ instance.vCPU }} vCPU
                              </div>
                              <div class="grid grid-cols-2 gap-2">
                                <UFormField
                                  :label="`Price/Hour ($)`"
                                  :name="`digitalocean-${cpuKey}-2nodes-hourly-${idx}`"
                                >
                                  <UInputNumber
                                    v-model="
                                      digitalOceanPricing.cpuOptions[cpuKey].instancesWith2Nodes[
                                        idx
                                      ]!.pricePerHour
                                    "
                                    :min="0"
                                    :max="10"
                                    :step="0.00001"
                                  />
                                </UFormField>
                                <UFormField
                                  :label="`Price/Month ($)`"
                                  :name="`digitalocean-${cpuKey}-2nodes-monthly-${idx}`"
                                >
                                  <UInputNumber
                                    v-model="
                                      digitalOceanPricing.cpuOptions[cpuKey].instancesWith2Nodes[
                                        idx
                                      ]!.pricePerMonth
                                    "
                                    :min="0"
                                    :max="10000"
                                    :step="0.01"
                                  />
                                </UFormField>
                              </div>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </template>
            </UCollapsible>

            <!-- Vercel Static IP Pricing -->
            <UCollapsible>
              <template #default="{ open }">
                <UButton
                  color="neutral"
                  variant="ghost"
                  class="w-full justify-between rounded-lg p-3"
                >
                  <span class="font-medium">Vercel Static IP Pricing</span>
                  <UIcon
                    :name="
                      open ? 'i-lucide-chevron-up' : 'i-lucide-chevron-down'
                    "
                    class="w-4 h-4"
                  />
                </UButton>
              </template>
              <template #content>
                <div class="grid grid-cols-2 gap-4 pt-4">
                  <UFormField
                    label="Monthly Fee ($)"
                    name="vercelStaticIPMonthly"
                  >
                    <UInputNumber
                      v-model="vercelStaticIPPricing.monthlyFee"
                      :min="0"
                      :max="1000"
                      :step="1"
                    />
                  </UFormField>
                  <UFormField
                    label="Private Data Transfer per GB ($)"
                    name="vercelStaticIPDataTransfer"
                  >
                    <UInputNumber
                      v-model="vercelStaticIPPricing.privateDataTransferPerGB"
                      :min="0"
                      :max="1"
                      :step="0.01"
                    />
                  </UFormField>
                </div>
              </template>
            </UCollapsible>
          </div>
        </UCard>
      </div>

      <!-- Results Panel -->
      <div class="lg:col-span-2">
        <!-- Detailed Breakdown -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <!-- AWS Aurora -->
          <UCard>
            <template #header>
              <div class="flex items-center justify-between">
                <div>
                  <h3 class="font-semibold">AWS Aurora Postgres</h3>
                  <p class="text-xs text-muted mt-1">
                    {{
                      inputs.auroraType === "serverless"
                        ? `Serverless (${
                            inputs.auroraServerlessConfig === "ioOptimized"
                              ? "I/O-Optimized"
                              : "Standard"
                          })`
                        : `Provisioned (${
                            inputs.auroraProvisionedConfig === "ioOptimized"
                              ? "I/O-Optimized"
                              : "Standard"
                          })`
                    }}
                  </p>
                </div>
                <UBadge color="primary" variant="subtle">
                  {{ formatCurrency(auroraCost.total) }}
                </UBadge>
              </div>
            </template>

            <div class="space-y-2 text-sm">
              <div class="flex justify-between">
                <span class="text-muted">
                  {{
                    inputs.auroraType === "serverless"
                      ? "ACU Capacity"
                      : "Instance"
                  }}
                </span>
                <span>
                  {{
                    formatCurrency(
                      inputs.auroraType === "serverless"
                        ? auroraCost.acu ?? 0
                        : auroraCost.instance ?? 0
                    )
                  }}
                </span>
              </div>
              <div class="flex justify-between">
                <span class="text-muted">Storage</span>
                <span>{{ formatCurrency(auroraCost.storage) }}</span>
              </div>
              <div
                v-if="
                  inputs.auroraType === 'provisioned' ||
                  (inputs.auroraType === 'serverless' &&
                    inputs.auroraServerlessConfig === 'standard')
                "
                class="flex justify-between"
              >
                <span class="text-muted">I/O Requests</span>
                <span>{{ formatCurrency(auroraCost.io) }}</span>
              </div>
              <div class="flex justify-between">
                <span class="text-muted">Backup Storage</span>
                <span>{{ formatCurrency(auroraCost.backup) }}</span>
              </div>
              <div class="flex justify-between">
                <span class="text-muted">Data Transfer</span>
                <span>{{ formatCurrency(auroraCost.dataTransfer) }}</span>
              </div>
              <div v-if="inputs.vercelStaticIP" class="flex justify-between">
                <span class="text-muted">Vercel Static IP</span>
                <span>{{ formatCurrency(auroraCost.vercelStaticIP) }}</span>
              </div>
              <USeparator />
              <div class="flex justify-between font-semibold">
                <span>Total</span>
                <span>{{ formatCurrency(auroraCost.total) }}</span>
              </div>
            </div>
          </UCard>

          <!-- Neon DB -->
          <UCard>
            <template #header>
              <div class="flex items-center justify-between">
                <div>
                  <h3 class="font-semibold">Neon DB</h3>
                  <p class="text-xs text-muted mt-1">
                    {{
                      inputs.neonTier.charAt(0).toUpperCase() +
                      inputs.neonTier.slice(1)
                    }}
                    Tier
                  </p>
                </div>
                <UBadge color="primary" variant="subtle">
                  {{ formatCurrency(neonCost.total) }}
                </UBadge>
              </div>
            </template>

            <div class="space-y-2 text-sm">
              <div class="flex justify-between">
                <span class="text-muted">Compute</span>
                <span>{{ formatCurrency(neonCost.compute) }}</span>
              </div>
              <div class="flex justify-between">
                <span class="text-muted">Storage</span>
                <span>{{ formatCurrency(neonCost.storage) }}</span>
              </div>
              <div class="flex justify-between">
                <span class="text-muted">PITR Storage</span>
                <span>{{ formatCurrency(neonCost.pitr) }}</span>
              </div>
              <div class="flex justify-between">
                <span class="text-muted">Data Transfer</span>
                <span>{{ formatCurrency(neonCost.dataTransfer) }}</span>
              </div>
              <div v-if="inputs.vercelStaticIP" class="flex justify-between">
                <span class="text-muted">Vercel Static IP</span>
                <span>{{ formatCurrency(neonCost.vercelStaticIP) }}</span>
              </div>
              <div
                v-if="
                  neonCost.total - neonCost.vercelStaticIP ===
                    neonPricing.tiers[inputs.neonTier].minimumMonthly &&
                  neonPricing.tiers[inputs.neonTier].minimumMonthly > 0
                "
                class="text-xs text-muted italic"
              >
                * Minimum ${{
                  neonPricing.tiers[inputs.neonTier].minimumMonthly
                }}/month applied
              </div>
              <USeparator />
              <div class="flex justify-between font-semibold">
                <span>Total</span>
                <span>{{ formatCurrency(neonCost.total) }}</span>
              </div>
            </div>
          </UCard>

          <!-- PlanetScale -->
          <UCard>
            <template #header>
              <div class="flex items-center justify-between">
                <h3 class="font-semibold">PlanetScale</h3>
                <UBadge color="primary" variant="subtle">
                  {{ formatCurrency(planetscaleCost.total) }}
                </UBadge>
              </div>
            </template>

            <div class="space-y-2 text-sm">
              <div class="flex justify-between">
                <span class="text-muted">Cluster</span>
                <span>{{ formatCurrency(planetscaleCost.cluster) }}</span>
              </div>
              <div class="flex justify-between">
                <span class="text-muted">Storage</span>
                <span>{{ formatCurrency(planetscaleCost.storage) }}</span>
              </div>
              <div class="flex justify-between">
                <span class="text-muted">Data Transfer</span>
                <span>{{ formatCurrency(planetscaleCost.dataTransfer) }}</span>
              </div>
              <div class="flex justify-between">
                <span class="text-muted">Branches</span>
                <span>{{ formatCurrency(planetscaleCost.branches) }}</span>
              </div>
              <div v-if="inputs.vercelStaticIP" class="flex justify-between">
                <span class="text-muted">Vercel Static IP</span>
                <span>{{
                  formatCurrency(planetscaleCost.vercelStaticIP)
                }}</span>
              </div>
              <USeparator />
              <div class="flex justify-between font-semibold">
                <span>Total</span>
                <span>{{ formatCurrency(planetscaleCost.total) }}</span>
              </div>
            </div>
          </UCard>

          <!-- Digital Ocean -->
          <UCard>
            <template #header>
              <div class="flex items-center justify-between">
                <div>
                  <h3 class="font-semibold">Digital Ocean PostgreSQL</h3>
                  <p class="text-xs text-muted mt-1">
                    {{
                      inputs.digitalOceanCPUOption === "basicRegular"
                        ? "Basic Regular"
                        : inputs.digitalOceanCPUOption === "basicPremiumIntel"
                        ? "Basic Premium Intel"
                        : inputs.digitalOceanCPUOption === "basicPremiumAMD"
                        ? "Basic Premium AMD"
                        : inputs.digitalOceanCPUOption === "generalPurpose"
                        ? "General Purpose"
                        : "Storage Optimized"
                    }}
                    • {{ digitalOceanInstance?.ram }} GiB RAM,
                    {{ digitalOceanInstance?.vCPU }} vCPU
                    {{
                      inputs.digitalOceanAdditionalNodes > 0
                        ? `+ ${inputs.digitalOceanAdditionalNodes} node(s)`
                        : ""
                    }}
                  </p>
                </div>
                <UBadge color="primary" variant="subtle">
                  {{ formatCurrency(digitalOceanCost.total) }}
                </UBadge>
              </div>
            </template>

            <div class="space-y-2 text-sm">
              <div class="flex justify-between">
                <span class="text-muted">Instance</span>
                <span>{{ formatCurrency(digitalOceanCost.instance) }}</span>
              </div>
              <div class="flex justify-between">
                <span class="text-muted">Storage</span>
                <span>{{ formatCurrency(digitalOceanCost.storage) }}</span>
              </div>
              <div class="flex justify-between">
                <span class="text-muted">Data Transfer</span>
                <span>{{ formatCurrency(digitalOceanCost.dataTransfer) }}</span>
              </div>
              <div v-if="inputs.vercelStaticIP" class="flex justify-between">
                <span class="text-muted">Vercel Static IP</span>
                <span>{{
                  formatCurrency(digitalOceanCost.vercelStaticIP)
                }}</span>
              </div>
              <USeparator />
              <div class="flex justify-between font-semibold">
                <span>Total</span>
                <span>{{ formatCurrency(digitalOceanCost.total) }}</span>
              </div>
            </div>
          </UCard>
        </div>

        <!-- Pricing Notes -->
        <UCard class="mt-6">
          <template #header>
            <h3 class="font-semibold">Pricing Notes</h3>
          </template>

          <div class="space-y-3 text-sm text-muted">
            <div>
              <strong>AWS Aurora Postgres:</strong> Serverless pricing based on
              Singapore region. Standard: $0.20/ACU-hour, $0.11/GB storage,
              $0.22/million I/O requests. I/O-Optimized: $0.26/ACU-hour,
              $0.248/GB storage, I/O included. Data transfer: first 1GB free,
              then $0.09/GB. Provisioned instances available with on-demand
              pricing.
            </div>
            <div>
              <strong>Neon DB:</strong> Scale tier pricing: $0.222/CU-hour,
              $0.35/GB storage, $0.20/GB PITR storage. Minimum $5/month spend.
              First 100GB data transfer included, then $0.10/GB. Compute Units
              scale automatically with scale-to-zero after 5 minutes of
              inactivity.
            </div>
            <div>
              <strong>PlanetScale:</strong> First 10GB storage included, then
              $0.45/GB. First 100GB data transfer included, then $0.096/GB.
              Branching included in HA plans. All HA clusters include 3 nodes (1
              primary + 2 replicas) across 3 availability zones.
            </div>
            <div>
              <strong>Digital Ocean PostgreSQL:</strong> Managed PostgreSQL
              database with multiple instance sizes. Storage pricing: $0.215/GiB
              per month. Inbound traffic is free; outbound (egress) traffic
              costs $0.01/GiB with 1000 GiB included per month for Managed
              Databases. Additional standby nodes available for high
              availability (0-2 nodes). Pricing based on Basic Regular CPU
              option.
            </div>
            <div v-if="inputs.vercelStaticIP">
              <strong>Vercel Static IP:</strong> $100/month per project plus
              $0.15/GB for private data transfer. Enables IP allowlisting for
              database access from Vercel deployments.
            </div>
          </div>
        </UCard>
      </div>
    </div>
  </UContainer>
</template>
