<script setup lang="ts">
// AWS Aurora Postgres pricing - editable
const auroraPricing = reactive({
  // Provisioned instances
  instances: [
    { name: "db.r5.large", vCPU: 2, ram: 16, pricePerHour: 0.33 },
    { name: "db.r5.xlarge", vCPU: 4, ram: 32, pricePerHour: 0.66 },
    { name: "db.r5.2xlarge", vCPU: 8, ram: 64, pricePerHour: 1.32 },
    { name: "db.r5.4xlarge", vCPU: 16, ram: 128, pricePerHour: 2.64 },
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
  auroraIORequests: number;
  auroraBackupGB: number;
  neonComputeUnits: number;
  neonPITRStorageGB: number;
  planetscaleClusterName: string;
  planetscaleBranches: number;
  auroraServerlessACU: number;
  auroraServerlessConfig: "standard" | "ioOptimized";
  neonTier: "free" | "launch" | "scale";
}>({
  // Common inputs
  storageGB: 100,
  dataTransferGB: 50,
  vercelStaticIP: false,

  // AWS Aurora
  auroraType: "serverless",
  auroraInstanceName: "db.r5.large",
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
});

// Computed instance/cluster objects
const auroraInstance = computed(() => {
  return (
    auroraPricing.instances.find((i) => i.name === inputs.auroraInstanceName) ||
    auroraPricing.instances[0]
  );
});

const planetscaleCluster = computed(() => {
  return (
    planetscalePricing.clusters.find(
      (c) => c.name === inputs.planetscaleClusterName
    ) || planetscalePricing.clusters[1]
  );
});

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
    // Assuming average ACU capacity for the month
    const acuCost = inputs.auroraServerlessACU * config.acuPerHour * 24 * 30;
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
    const instance = auroraInstance.value!;
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
  const totalCUHours = inputs.neonComputeUnits * 24 * 30;
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
              </div>
            </div>

            <!-- AWS Aurora -->
            <div>
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
                    label="I/O Requests (millions/month)"
                    name="auroraIO"
                  >
                    <UInputNumber
                      v-model="inputs.auroraIORequests"
                      :min="0"
                      :max="1000"
                      :step="0.1"
                    />
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

            <!-- Neon DB -->
            <div>
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

            <!-- PlanetScale -->
            <div>
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
                          <div class="font-medium">{{ instance.name }}</div>
                          <div class="grid grid-cols-3 gap-2">
                            <UFormField
                              :label="`${instance.name} Price/Hour ($)`"
                              :name="`aurora-instance-${idx}`"
                            >
                              <UInputNumber
                                v-model="auroraPricing.instances[idx]!.pricePerHour"
                                :min="0"
                                :max="100"
                                :step="0.01"
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
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
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
                        : "Provisioned"
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
