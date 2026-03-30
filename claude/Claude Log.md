Claude Log


base_bert.py
Read this repository, summarize what the project is intended to do,how it should be run, and which APIs, commands, and libraries are used.
Let me explore the assignment2 directory to understand the project.

Bash
IN
ls /Users/salmaelotmani/SWE\ Project/cs769-assignments/assignment2/

OUT
LICENSE
README.md
base_bert.py
bert.py
classifier.py
config.py
data
demo
optimizer.py
optimizer_test.npy
optimizer_test.py
prepare_submit.py
run_exp.sh
sanity_check.data
sanity_check.py
setup.sh
structure.md
tokenizer.py
utils.py
Read README.md
Read structure.md
Read run_exp.sh
Read setup.sh
Read classifier.py, bert.py
Here's a summary of the project:

Assignment 2: minBERT — Minimalist BERT Implementation
Purpose
A CS769 (NLP) homework assignment to implement key components of BERT from scratch and use it for sentence classification on two datasets:

SST (Stanford Sentiment Treebank) — 5-class sentiment analysis
CFIMDB — binary sentiment classification
Students implement the core BERT internals using only PyTorch (no transformers library allowed), then load pretrained bert-base-uncased weights from HuggingFace for fine-tuning.

Files & What to Implement
File	Role	Status
bert.py	Core BERT model (attention, layers, embeddings)	TODO: BertSelfAttention.attention, BertLayer.add_norm, BertLayer.forward, BertModel.embed
classifier.py	BertSentClassifier + training/eval pipeline	TODO: BertSentClassifier.__init__ and forward
optimizer.py	AdamW optimizer	TODO: step() method
base_bert.py	Weight loading from HuggingFace	Pre-implemented
tokenizer.py	BertTokenizer	Pre-implemented
config.py	BERT config dataclass	Pre-implemented
utils.py	Attention mask helpers etc.	Pre-implemented
How to Run
Environment setup:


conda create -n bert_hw python=3.7
conda activate bert_hw
# then run setup.sh
bash setup.sh
Train & evaluate:


# Pretrain mode (frozen BERT weights, only classifier head trained):
python classifier.py --option pretrain --epochs 10 --lr 1e-3 \
    --train data/sst-train.txt --dev data/sst-dev.txt --test data/sst-test.txt

# Finetune mode (all BERT weights updated):
python classifier.py --option finetune --epochs 10 --lr 1e-5 \
    --train data/sst-train.txt --dev data/sst-dev.txt --test data/sst-test.txt
Sanity checks (unit tests):


python sanity_check.py       # tests BERT implementation
python optimizer_test.py     # tests AdamW implementation
Libraries & APIs Used
Library	Use
torch / torch.nn / torch.nn.functional	All model building, training, loss computation
torch.utils.data (Dataset, DataLoader)	Data pipeline
sklearn.metrics	accuracy_score, f1_score for evaluation
tqdm	Training progress bars
tokenizers (HuggingFace)	Underlying tokenizer for BertTokenizer
numpy	Array ops, random seed management
argparse	CLI argument parsing
Pre-trained weights are loaded from HuggingFace's bert-base-uncased model via the custom BertPreTrainedModel.from_pretrained() in base_bert.py.

config.py
how many todos/noimplemented stuff?
Let me search for TODOs and NotImplementedError in the assignment2 directory.

Grep "todo|NotImplementedError|raise NotImplemented" (in /Users/salmaelotmani/SWE Project/cs769-assignments/assignment2, glob: *.py)
29 lines of output
Ignoring tokenizer.py (pre-implemented, not for students to touch), there are 6 student TODOs across 3 files:

File	Line	What
bert.py:50	BertSelfAttention.attention	Multi-head scaled dot-product attention
bert.py:92-93	BertLayer.add_norm	LayerNorm(input + Dropout(Dense(output)))
bert.py:105-115	BertLayer.forward	Full transformer block (attn → add_norm → FFN → add_norm)
bert.py:154-173	BertModel.embed	Word + position + token-type embeddings
classifier.py:40-41	BertSentClassifier.__init__	Dropout + linear classification head
classifier.py:44-46	BertSentClassifier.forward	Encode with BERT, classify from [CLS] pooler output
optimizer.py:41	AdamW.step	AdamW update rule with decoupled weight decay
7 total raise NotImplementedError blocks that need to be filled in.

optimizer.py#41-41
Now try to run the project or its tests. Before making any edits, identify the unfinished / TODO / NotImplementedError functions, and count and list exactly where they are (file and function name). Show me the exact commands you want to run, then report the output and any errors. Do not edit files yet.
The user wants me to:

List exactly where the TODOs/NotImplementedError are (file and function name)
Show the exact commands to run
Report output and errors
Do NOT edit files yet
I already have a good picture from the previous analysis. Let me confirm by reading the optimizer.py file too.

Read optimizer.py
Exact TODO / NotImplementedError locations
#	File	Function	Line	What's missing
1	bert.py	BertSelfAttention.attention	50	Scaled dot-product attention + multi-head concat
2	bert.py	BertLayer.add_norm	93	LayerNorm(input + Dropout(Dense(output)))
3	bert.py	BertLayer.forward	115	Full transformer block (attn → add_norm → FFN → add_norm)
4	bert.py	BertModel.embed	173	Word + positional embeddings lookup & sum
5	classifier.py	BertSentClassifier.__init__	41	Dropout + linear classification head
6	classifier.py	BertSentClassifier.forward	46	Encode with BERT, classify from [CLS] token
7	optimizer.py	AdamW.step	41	AdamW update rule (moments, bias correction, weight decay)
Total: 7 unimplemented blocks

Commands to run

# Test 1: BERT sanity check
cd "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2"
python sanity_check.py

# Test 2: Optimizer test
python optimizer_test.py

# Test 3: Full training (pretrain mode, small run)
python classifier.py --option pretrain --epochs 1 --lr 1e-3 \
    --train data/sst-train.txt --dev data/sst-dev.txt --test data/sst-test.txt
Bash Run BERT sanity check
IN
cd "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2" && python sanity_check.py 2>&1

OUT
Exit code 127
(eval):1: command not found: python
Bash Run BERT sanity check with python3
IN
cd "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2" && python3 sanity_check.py 2>&1

OUT
Exit code 1
Traceback (most recent call last):
  File "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2/sanity_check.py", line 1, in <module>
    import torch
ModuleNotFoundError: No module named 'torch'
Bash Run optimizer test with python3
IN
cd "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2" && python3 optimizer_test.py 2>&1

OUT
<tool_use_error>Cancelled: parallel tool call Bash(cd "/Users/salmaelotmani/SWE Project/cs7…) errored</tool_use_error>
Bash Check if bert_hw conda env exists and has torch
IN
conda run -n bert_hw python3 --version 2>&1 && conda run -n bert_hw python3 -c "import torch; print(torch.__version__)" 2>&1

OUT
Exit code 127
(eval):1: command not found: conda
Bash Find available Python environments
IN
which python3; python3 -c "import sys; print(sys.executable)"; ls ~/anaconda3/envs/ 2>/dev/null || ls ~/miniconda3/envs/ 2>/dev/null || ls ~/opt/anaconda3/envs/ 2>/dev/null || echo "no conda envs found"

OUT
/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
no conda envs found
Bash Search for conda installation
IN
find /usr/local /opt /Users/salmaelotmani -name "conda" -o -name "activate" 2>/dev/null | grep -E "conda$|activate$" | head -10

OUT
/Users/salmaelotmani/SWE Project/cs769-assignments/assignment1/venv/bin/activate
/Users/salmaelotmani/SWE Project/sp26-hw1-bash-only/.venv/bin/activate
/Users/salmaelotmani/SWE Project/sp26-hw3-skills/.venv/bin/activate
/Users/salmaelotmani/SWE Project/sp26-hw2-tool-use/.venv/bin/activate
/Users/salmaelotmani/.local/share/uv/tools/prek/bin/activate
/Users/salmaelotmani/.local/share/uv/python/cpython-3.12.12-macos-aarch64-none/lib/python3.12/venv/scripts/common/activate
/Users/salmaelotmani/pl-uiuc-csed-dev/.venv/bin/activate
/Users/salmaelotmani/sp26-hw1-bash-only/.venv/bin/activate
/Users/salmaelotmani/sp26-hw3-skills/.venv/bin/activate
/Users/salmaelotmani/.cache/prek/hooks/python-IIN1K8sD0xKWvtayVqvH/bin/activate
Bash Check if assignment1 venv has torch
IN
source "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment1/venv/bin/activate" && python3 -c "import torch; print('torch:', torch.__version__)" 2>&1

OUT
torch: 2.8.0
Bash Run sanity_check using assignment1 venv
IN
cd "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2" && source "../assignment1/venv/bin/activate" && python3 sanity_check.py 2>&1

OUT
Exit code 1
/Users/salmaelotmani/SWE Project/cs769-assignments/assignment1/venv/lib/python3.9/site-packages/urllib3/__init__.py:35: NotOpenSSLWarning: urllib3 v2 only supports OpenSSL 1.1.1+, currently the 'ssl' module is compiled with 'LibreSSL 2.8.3'. See: https://github.com/urllib3/urllib3/issues/3020
  warnings.warn(
Traceback (most recent call last):
  File "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2/sanity_check.py", line 24, in <module>
    embed_outputs = bert.embed(input_ids=sent_ids)
  File "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2/bert.py", line 167, in embed
    embeds = inputs_embeds + tk_type_embeds + pos_embeds
TypeError: unsupported operand type(s) for +: 'NoneType' and 'Tensor'
Bash Run optimizer_test using assignment1 venv
IN
cd "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2" && source "../assignment1/venv/bin/activate" && python3 optimizer_test.py 2>&1

OUT
<tool_use_error>Cancelled: parallel tool call Bash(cd "/Users/salmaelotmani/SWE Project/cs7…) errored</tool_use_error>
Bash Run optimizer_test
IN
cd "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2" && source "../assignment1/venv/bin/activate" && python3 optimizer_test.py 2>&1

OUT
Exit code 1
Traceback (most recent call last):
  File "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2/optimizer_test.py", line 30, in <module>
    actual = test_optimizer(AdamW)
  File "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2/optimizer_test.py", line 25, in test_optimizer
    opt.step()
  File "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment1/venv/lib/python3.9/site-packages/torch/optim/optimizer.py", line 516, in wrapper
    out = func(*args, **kwargs)
  File "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2/optimizer.py", line 41, in step
    raise NotImplementedError()
NotImplementedError
Results
Python environment: No conda; using assignment1/venv (Python 3.9, torch 2.8.0).

python3 sanity_check.py

TypeError: unsupported operand type(s) for +: 'NoneType' and 'Tensor'
  File "bert.py", line 167, in embed
    embeds = inputs_embeds + tk_type_embeds + pos_embeds
Fails immediately in BertModel.embed — inputs_embeds and pos_embeds are None because the word/position embedding lookups are not implemented (TODOs #1 and #4). The test never reaches the attention or layer-norm code.

python3 optimizer_test.py

NotImplementedError
  File "optimizer.py", line 41, in step
    raise NotImplementedError()
Fails immediately — AdamW.step hits the bare raise NotImplementedError() before doing anything (TODO #7).

python3 classifier.py --option pretrain ...
Would fail the same way as sanity_check.py (hits BertModel.embed first), so not worth running separately until the BERT TODOs are filled in.

Bottom line: All 7 TODOs are confirmed unimplemented and all tests fail at their first unimplemented block.

Now implement the unfinished/ToDo functions with the smallest possible changes needed to get the project working. Do not create any new files unless absolutely necessary. Explain each change before or as you make it, and avoid rewriting the whole project. Explain which APIs, commands, and libraries you are using. Do not modify other files unless absolutely necessary.
Let me implement the 7 unfinished functions one by one. Let me read each file carefully before making changes.

I already have the content of bert.py, classifier.py, and optimizer.py from earlier reads. Let me implement each TODO:

BertModel.embed (bert.py:149-173) - Word + positional embeddings lookup

BertSelfAttention.attention (bert.py:37-50) - Scaled dot-product attention

BertLayer.add_norm (bert.py:84-93) - LayerNorm(input + Dropout(Dense(output)))

BertLayer.forward (bert.py:95-115) - Full transformer block

BertSentClassifier.__init__ (classifier.py:27-41) - Dropout + linear head

BertSentClassifier.forward (classifier.py:43-46) - Encode + classify

AdamW.step (optimizer.py:28-41) - AdamW update rule

Now I'm planning out each implementation, starting with the embedding layer that combines word and positional embeddings.

Implementing the attention mechanism... Continuing through the BertLayer forward pass, I apply the feed-forward network with an intermediate dense layer and activation function, then another add-norm operation to produce the final output. For the classifier, I'm setting up dropout and a linear layer that maps from the hidden size to the number of labels, then in the forward method I extract the pooler output from the BERT model...

Applying dropout and passing through the classifier to get logits, which I convert to log probabilities. Now looking at the AdamW optimizer step, specifically the efficient version from the Adam paper where I compute the adaptive learning rate using bias correction terms for both the first and second moment estimates, then update the parameters using the momentum and variance terms.

Initializing the optimizer state with step counter and exponential moving averages for gradients and squared gradients, then updating these moments with the current gradient before applying bias correction and weight decay to the parameter update.

Now I'm working through the file edits systematically, starting with the bert.py changes where I need to replace the NotImplementedError and fill in the embedding function properly.

Update Todos
IN
{
  "todos": "[\n  {\"id\": \"1\", \"content\": \"bert.py: BertModel.embed — word + positional embedding lookups\", \"status\": \"in_progress\", \"priority\": \"high\"},\n  {\"id\": \"2\", \"content\": \"bert.py: BertSelfAttention.attention — scaled dot-product attention\", \"status\": \"pending\", \"priority\": \"high\"},\n  {\"id\": \"3\", \"content\": \"bert.py: BertLayer.add_norm — LayerNorm(input + Dropout(Dense(output)))\", \"status\": \"pending\", \"priority\": \"high\"},\n  {\"id\": \"4\", \"content\": \"bert.py: BertLayer.forward — full transformer block\", \"status\": \"pending\", \"priority\": \"high\"},\n  {\"id\": \"5\", \"content\": \"classifier.py: BertSentClassifier.__init__ — dropout + linear head\", \"status\": \"pending\", \"priority\": \"high\"},\n  {\"id\": \"6\", \"content\": \"classifier.py: BertSentClassifier.forward — encode + classify\", \"status\": \"pending\", \"priority\": \"high\"},\n  {\"id\": \"7\", \"content\": \"optimizer.py: AdamW.step — AdamW update rule\", \"status\": \"pending\", \"priority\": \"high\"}\n]"
}
OUT
<tool_use_error>InputValidationError: TodoWrite failed due to the following issue:
The parameter `todos` type is expected as `array` but provided as `string`

This tool's schema was not sent to the API — it was not in the discovered-tool set derived from message history. Without the schema in your prompt, typed parameters (arrays, numbers, booleans) get emitted as strings and the client-side parser rejects them. Load the tool first: call ToolSearch with query "select:TodoWrite", then retry this call.</tool_use_error>
Now let me implement all 7 TODOs. Let me do them in order, starting with bert.py.

Update Todos

bert.py: BertModel.embed — word + positional embedding lookups

bert.py: BertSelfAttention.attention — scaled dot-product attention

bert.py: BertLayer.add_norm — LayerNorm(input + Dropout(Dense(output)))

bert.py: BertLayer.forward — full transformer block

classifier.py: BertSentClassifier.__init__ and forward

optimizer.py: AdamW.step — AdamW update rule
1. BertModel.embed — bert.py:149-173
The two None assignments need to call self.word_embedding(input_ids) and self.pos_embedding(pos_ids) (both are torch.nn.Embedding layers — just call them like functions). Then remove the raise NotImplementedError and return the result.

Edit bert.py
Removed 2 lines
Edit bert.py
Modified
The diagnostics are just warnings/hints about torch not being resolvable (because torch isn't installed in the default Python environment, but it is in the venv) and unused imports. These are not real errors - they're just IDE warnings. I can ignore these and continue with the implementations.

Let me continue implementing the remaining TODOs.

The IDE warnings are just Pylance not finding torch in the default Python path (not the venv) — not real errors. Continuing.

Update Todos

bert.py: BertModel.embed — word + positional embedding lookups

bert.py: BertSelfAttention.attention — scaled dot-product attention

bert.py: BertLayer.add_norm — LayerNorm(input + Dropout(Dense(output)))

bert.py: BertLayer.forward — full transformer block

classifier.py: BertSentClassifier.__init__ and forward

optimizer.py: AdamW.step — AdamW update rule
2. BertSelfAttention.attention — bert.py:37
Implements the formula Attention(Q,K,V) = softmax(QKᵀ / √dₖ) · V.

torch.matmul does batched matrix multiply
divide by math.sqrt(self.attention_head_size) for scaling (dₖ)
add attention_mask directly (non-padding = 0, padding = large negative → softmax kills them)
F.softmax over the last dim (key positions), then apply dropout
transpose(1,2).contiguous().view(...) merges the heads back into hidden_size
Edit bert.py
Added 8 lines
Update Todos

bert.py: BertModel.embed — word + positional embedding lookups

bert.py: BertSelfAttention.attention — scaled dot-product attention

bert.py: BertLayer.add_norm — LayerNorm(input + Dropout(Dense(output)))

bert.py: BertLayer.forward — full transformer block

classifier.py: BertSentClassifier.__init__ and forward

optimizer.py: AdamW.step — AdamW update rule
3. BertLayer.add_norm — bert.py:84
The docstring says: LayerNorm(input + Sublayer(output)) where Sublayer = dense_layer followed by dropout. One line.

Edit bert.py
Removed 1 line
Update Todos

bert.py: BertModel.embed — word + positional embedding lookups

bert.py: BertSelfAttention.attention — scaled dot-product attention

bert.py: BertLayer.add_norm — LayerNorm(input + Dropout(Dense(output)))

bert.py: BertLayer.forward — full transformer block

classifier.py: BertSentClassifier.__init__ and forward

optimizer.py: AdamW.step — AdamW update rule
4. BertLayer.forward — bert.py:95
Wires together the transformer block per Figure 1 of "Attention is All You Need": self-attention → add_norm → feed-forward (dense + GELU) → add_norm. Uses the existing self.* layers already defined in __init__.

Edit bert.py
Removed 2 lines
Update Todos

bert.py: BertModel.embed — word + positional embedding lookups

bert.py: BertSelfAttention.attention — scaled dot-product attention

bert.py: BertLayer.add_norm — LayerNorm(input + Dropout(Dense(output)))

bert.py: BertLayer.forward — full transformer block

classifier.py: BertSentClassifier.__init__ and forward

optimizer.py: AdamW.step — AdamW update rule
5 & 6. BertSentClassifier.__init__ and forward — classifier.py:27
__init__: adds a torch.nn.Dropout layer and a torch.nn.Linear projection from hidden_size (768) → num_labels.

forward: calls self.bert(input_ids, attention_mask) which returns a dict; uses pooler_output (the [CLS] token embedding), applies dropout, linear, then F.log_softmax (the training loop uses F.nll_loss which expects log-probabilities).

Edit classifier.py
Added 2 lines
Update Todos

bert.py: BertModel.embed — word + positional embedding lookups

bert.py: BertSelfAttention.attention — scaled dot-product attention

bert.py: BertLayer.add_norm — LayerNorm(input + Dropout(Dense(output)))

bert.py: BertLayer.forward — full transformer block

classifier.py: BertSentClassifier.__init__ and forward

optimizer.py: AdamW.step — AdamW update rule
7. AdamW.step — optimizer.py:28
Implements the "efficient" AdamW from Kingma & Ba (2014) + Loshchilov & Hutter (2017):

torch.zeros_like initializes moment tensors in self.state[p] (PyTorch optimizer state dict)
exp_avg.mul_(beta1).add_(grad, alpha=1-beta1) — in-place update of first moment m_t
exp_avg_sq.mul_(beta2).addcmul_(grad, grad, value=1-beta2) — second moment v_t
Bias correction uses the "efficient" form: step_size = lr * sqrt(1-β₂ᵗ) / (1-β₁ᵗ) (single corrected LR, no m̂/v̂)
p.data.addcdiv_(exp_avg, sqrt(v) + eps, value=-step_size) — parameter update
Weight decay is applied after, scaled by lr (decoupled, as per AdamW)
Edit optimizer.py
Added 17 lines
Now run both tests:

Bash Run BERT sanity check
IN
cd "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2" && source "../assignment1/venv/bin/activate" && python3 sanity_check.py 2>&1

OUT
/Users/salmaelotmani/SWE Project/cs769-assignments/assignment1/venv/lib/python3.9/site-packages/urllib3/__init__.py:35: NotOpenSSLWarning: urllib3 v2 only supports OpenSSL 1.1.1+, currently the 'ssl' module is compiled with 'LibreSSL 2.8.3'. See: https://github.com/urllib3/urllib3/issues/3020
  warnings.warn(
Your BertModel.embed() implementation is correct!
Your BertSelfAttention implementation is correct!
Your BertLayer implementation is correct!
Your BERT implementation is correct!
Bash Run optimizer test
IN
cd "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2" && source "../assignment1/venv/bin/activate" && python3 optimizer_test.py 2>&1

OUT
Exit code 1
Traceback (most recent call last):
  File "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2/optimizer_test.py", line 30, in <module>
    actual = test_optimizer(AdamW)
  File "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2/optimizer_test.py", line 25, in test_optimizer
    opt.step()
  File "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment1/venv/lib/python3.9/site-packages/torch/optim/optimizer.py", line 516, in wrapper
    out = func(*args, **kwargs)
  File "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2/optimizer.py", line 67, in step
    step_size = alpha * math.sqrt(bias_correction2) / bias_correction1
NameError: name 'math' is not defined
I need to import math in optimizer.py. Let me check the imports.

math isn't imported in optimizer.py. Need to add it:

Edit optimizer.py
Added 1 line
Again just Pylance/venv path warnings, not real errors. Re-running tests:

Bash Run optimizer test
IN
cd "/Users/salmaelotmani/SWE Project/cs769-assignments/assignment2" && source "../assignment1/venv/bin/activate" && python3 optimizer_test.py 2>&1

OUT
Optimizer test passed!
Both tests pass. Here's a summary of all changes made:

Changes summary
bert.py — 4 functions:

Function	What was done
BertModel.embed	Called self.word_embedding(input_ids) and self.pos_embedding(pos_ids) (nn.Embedding forward calls), replaced raise with return embeds
BertSelfAttention.attention	torch.matmul(Q, Kᵀ) / √dₖ + mask add, F.softmax, dropout, matmul(weights, V), then transpose+view to merge heads
BertLayer.add_norm	Single line: ln_layer(input + dropout(dense_layer(output)))
BertLayer.forward	Wired: self_attention → add_norm → interm_dense+GELU → add_norm, returning output
classifier.py — 2 functions:

Function	What was done
BertSentClassifier.__init__	Added torch.nn.Dropout and torch.nn.Linear(hidden_size, num_labels)
BertSentClassifier.forward	Calls self.bert(...)['pooler_output'], applies dropout + linear + F.log_softmax (needed because training uses F.nll_loss)
optimizer.py — 1 function + 1 import:

Change	What was done
AdamW.step	Initializes state with step counter + zero moment tensors; updates m/v in-place with mul_+add_/addcmul_; computes bias-corrected step_size = lr·√(1-β₂ᵗ)/(1-β₁ᵗ); updates params with addcdiv_; applies decoupled weight decay scaled by lr
import math	Required for math.sqrt in bias correction
