https://explorer.public.zkevm-test.net/tx/0x3afc3976984601dbefa24d1ba9b12947d1f6e95b78c4004c6fb5fbb91bd6ea11

const counterAddress = "0x3aC587078b344a3d27e56632dFf236F1Aff04D56";
console.log(counterAddress, "Counter ABI: ", Counter.abi);

function App() {
const [isLoading, setIsLoading] = useState(false);

async function requestAccount() {
await window.ethereum.request({ method: "eth_requestAccounts" });
}

async function updateNameInContract() {
if (typeof window.ethereum !== "undefined") {
await requestAccount();
const provider = new ethers.providers.Web3Provider(window.ethereum);
console.log({ provider });
const signer = provider.getSigner();
const contract = new ethers.Contract(counterAddress, Counter.abi, signer);
const transaction = await contract.submitUsername("pbrain19");
setIsLoading(true);
await transaction.wait();
setIsLoading(false);
}
}

const updateName = async () => {
await updateNameInContract();
};

return (
<Container maxWidth="sm">
<Card sx={{ minWidth: 275, marginTop: 20 }}>
<CardContent>
<Button onClick={updateName} variant="outlined" disabled={isLoading}>
{isLoading ? "loading..." : "+1"}
</Button>
</CardContent>
</Card>
</Container>
);
}

export default App;
