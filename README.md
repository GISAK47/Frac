# Frac
import * as ed from '@noble/ed25519';
import {
  ID_GATEWAY_ADDRESS,
  ID_REGISTRY_ADDRESS,
  ViemLocalEip712Signer,
  ViemEip712Signer,
  idGatewayABI,
  idRegistryABI,
  NobleEd25519Signer,
  BUNDLER_ADDRESS,
  bundlerABI,
} from '@farcaster/hub-nodejs';
import { bytesToHex, createPublicClient, createWalletClient, http } from 'viem';
import { privateKeyToAccount } from 'viem/accounts';
import { optimism } from 'viem/chains';

const APP_PRIVATE_KEY = '0x00';
const ALICE_PRIVATE_KEY = '0x00';

const publicClient = createPublicClient({
  chain: optimism,
  transport: http(),
});

const walletClient = createWalletClient({
  chain: optimism,
  transport: http(),
});

const app = privateKeyToAccount(APP_PK);
const accountKey = new ViemLocalEip712Signer(app);

const alice = privateKeyToAccount(ALICE_PK);
const aliceAccountKey = new ViemLocalEip712Signer(alice);

const getDeadline = () => {
  const now = Math.floor(Date.now() / 1000);
  const oneHour = 60 * 60;
  return BigInt(now + oneHour);
};

const WARPCAST_RECOVERY_PROXY = '0x00000000FcB080a4D6c39a9354dA9EB9bC104cd7';
let nonce = await publicClient.readContract({
  address: KEY_GATEWAY_ADDRESS,
  abi: keyGatewayABI,
  functionName: 'nonces',
  args: [alice.address],
});

const registerSignature = await aliceAccountKey.signRegister({
  to: alice.address,
  recovery: WARPCAST_RECOVERY_PROXY,
  nonce,
  deadline,
});
if (signedKeyRequestMetadata.isOk()) {
  const metadata = bytesToHex(signedKeyRequestMetadata.value);

  nonce = await publicClient.readContract({
    address: KEY_GATEWAY_ADDRESS,
    abi: keyGatewayABI,
    functionName: 'nonces',
    args: [alice.address],
  });

  const addSignature = await aliceAccountKey.signAdd({
    owner: alice.address,
    keyType: 1,
    key: accountPubKey,
    metadataType: 1,
    metadata,
    nonce,
    deadline,
  });
}
