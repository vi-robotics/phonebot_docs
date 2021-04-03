# Simulation

This is the documentation phase for the simulation related developments on the `Phonebot` platform.

We provide a `gym.Env`-compatible environment at `phonebot.sim.pybullet.simulator#PybulletPhonebotEnv`

## Backend Agnostic Simulation

Our simulation is reliant on the backend environment model definition. Whereas formats such as `URDF`s or `SDF`s would be typically used for this purpose, `URDF`s do not support closed-loop kinematic chains and `SDF` formats are largely constrained to `Gazebo` environments(much like `MJCF`s are local to `Mujoco` environments) and do not benefit from `xacro`-style composition that `URDF`s readily lend itself to.

In order to overcome these limitations we instead choose to programmatically define our robot through generic POD classes such as `sim.common.model#ModelLink` that can be easily hierarchically composed and instantiated programmatically. We connect such a model to a builder such as `PybulletBuilder` that preprocesses the model and converts it into the specific simulator with all its quirks. For instance, in `pybullet` the joints and links are the same thing (i.e. a tree layout), etc.

For now, however, we have concentrated our developments to `PyBullet`. Therefore, look to the `PybulletPhonebotEnv` for the most mature state of simulation development.

## Simulator Architecture

As an example, `PyBulletPhonbotEnv` largely consists of the following modular components:

- Robot.Sensor: Any combination of {`JointStateSensor`,`BasePoseSensor`, `PybulletPhonebotSensor`}
- [TODO] Robot.Controller: Any one of {`JointController`, `EndpointController`}
- Task: Any one of {`ForwardVelocityTask`, `HoldVelocityTask`, and `ReachPositionTask`}
- Environment: Currently only supports `PlaneEnvironment` which is an environment of an infinite flat plane.

I think this type of architecture, whether implemented as a `mixin` or a composition, readily lends itself to the following set of conditions:

- Performing different tasks with the same robot in the same environment
- Performing a single task in multiple different environment
- Performing the same task with different sensory inputs

## Reinforcement Learning

One particular use-case for a well-constructed and stable simulation is to try to tune parameters - one such application is the use of RL to optimize our gaits. For instance:

### Training and Evaluation

We mainly employ [stable_baselines3](https://github.com/DLR-RM/stable-baselines3) which is a `PyTorch` re-implementation of the `stable_baselines` and the lineage of work regarding establishing stable models for the various `gym.Env` environments.

Specifically, refer to the [yycho0108/stable_baselines3:v0.10.0-phonebot](https://github.com/yycho0108/rl-baselines-zoo/tree/v0.10.0-phonebot) fork which applies some custom modifications for a slightly more convenient training.

```bash
python3 train.py --algo ppo --env phonebot-pybullet-headless-v0 --gym-packages phonebot.sim.pybullet.simulator --save-freq 50000 -tb /tmp/ppo-log/
```

Note that the `phonebot` package needs to be setup through commands such as `pip install -e .`. Refer to the main installation guide for details on how to configure the package.

Likewise, the trained agent can be evaluated as follows:

```bash
python3 enjoy.py -f logs --exp-id 22 --algo ppo --gym-packages phonebot.sim.pybullet.simulator --env 'phonebot-pybullet-v0' --load-best -n 1000
```

However, since we have trained in a `headless` environment, running the above command naively would not work. Among other ways to make this work, one workaround is to employ symlinks as follows :

```bash
ln -s logs/ppo/phonebot-pybullet-headless-v0_${EXP_ID} logs/ppo/phonebot-pybullet-v0_${EXP_ID}
ln -s logs/ppo/phonebot-pybullet-v0_${EXP_ID}/phonebot-pybullet-headless-v0 logs/ppo/phonebot-pybullet-v0_${EXPI_ID}/phonebot-pybullet-v0
```

Note that `${EXP_ID}` in the above command refers to the experiment id (the same as `--exp-id` flag in `enjoy.py`, etc.)

### Result

Images will be added once the pending PR is merged. In the meantime, refer to the [Gdrive](https://drive.google.com/drive/folders/1HPd-6gB63BS3R0GvQg1SYY9ydZUGZOgb).

Trained Agent:

Learned Gait Policy:

Statistics collected during training (visualization produced in `Tensorboard` e.g. `tensorboard --logdir /tmp/ppo-log/`):
